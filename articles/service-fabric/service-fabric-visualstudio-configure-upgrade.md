---
title: Konfigurera uppgraderingen av ett Service Fabric-program | Microsoft Docs
description: "Lär dig hur du konfigurerar inställningar för att uppgradera ett Service Fabric-program med hjälp av Microsoft Visual Studio."
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
ms.openlocfilehash: 314b29a56e4651222822f40a116af97a7372ff2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="0718b-103">Konfigurera uppgraderingen av ett Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0718b-103">Configure the upgrade of a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="0718b-104">Visual Studio tools för Azure Service Fabric stödja uppgradering för publicering till lokal eller fjärransluten kluster.</span><span class="sxs-lookup"><span data-stu-id="0718b-104">Visual Studio tools for Azure Service Fabric provide upgrade support for publishing to local or remote clusters.</span></span> <span data-ttu-id="0718b-105">Det finns tre scenarier som du vill uppgradera ditt program till en nyare version i stället för att ersätta program under testning och felsökning:</span><span class="sxs-lookup"><span data-stu-id="0718b-105">There are three scenarios in which you want to upgrade your application to a newer version instead of replacing the application during testing and debugging:</span></span>

* <span data-ttu-id="0718b-106">Programmet data förloras inte under uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="0718b-106">Application data won't be lost during the upgrade.</span></span>
* <span data-ttu-id="0718b-107">Tillgänglighet fortfarande hög så det finns inte några avbrott i tjänsten under uppgraderingen, om det finns tillräckligt med instanser av tjänsten sprids uppgraderingsdomäner.</span><span class="sxs-lookup"><span data-stu-id="0718b-107">Availability remains high so there won't be any service interruption during the upgrade, if there are enough service instances spread across upgrade domains.</span></span>
* <span data-ttu-id="0718b-108">Du kan köra testerna mot ett program medan den uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="0718b-108">Tests can be run against an application while it's being upgraded.</span></span>

## <a name="parameters-needed-to-upgrade"></a><span data-ttu-id="0718b-109">Parametrar som behövs för att uppgradera</span><span class="sxs-lookup"><span data-stu-id="0718b-109">Parameters needed to upgrade</span></span>
<span data-ttu-id="0718b-110">Du kan välja mellan två typer av distribution: regelbundna eller uppgradering.</span><span class="sxs-lookup"><span data-stu-id="0718b-110">You can choose from two types of deployment: regular or upgrade.</span></span> <span data-ttu-id="0718b-111">En vanlig distribution raderas alla tidigare distributionsinformation och data på klustret, medan en uppgradering distribution bevarar den.</span><span class="sxs-lookup"><span data-stu-id="0718b-111">A regular deployment erases any previous deployment information and data on the cluster, while an upgrade deployment preserves it.</span></span> <span data-ttu-id="0718b-112">Du måste ange parametrar för uppgradering av program och hälsa kontrollera principer när du uppgraderar ett Service Fabric-program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0718b-112">When you upgrade a Service Fabric application in Visual Studio, you need to provide application upgrade parameters and health check policies.</span></span> <span data-ttu-id="0718b-113">Uppgradera parametrar för program för att förhindra uppgraderingen vid kontroll av hälsoprinciper fastställa om uppgraderingen lyckades.</span><span class="sxs-lookup"><span data-stu-id="0718b-113">Application upgrade parameters help control the upgrade, while health check policies determine whether the upgrade was successful.</span></span> <span data-ttu-id="0718b-114">Se [uppgradering av Service Fabric-programmet: Uppgraderingsparametrar](service-fabric-application-upgrade-parameters.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="0718b-114">See [Service Fabric application upgrade: upgrade parameters](service-fabric-application-upgrade-parameters.md) for more details.</span></span>

<span data-ttu-id="0718b-115">Det finns tre uppgradera lägen: *övervakade*, *UnmonitoredAuto*, och *UnmonitoredManual*.</span><span class="sxs-lookup"><span data-stu-id="0718b-115">There are three upgrade modes: *Monitored*, *UnmonitoredAuto*, and *UnmonitoredManual*.</span></span>

* <span data-ttu-id="0718b-116">En uppgradering av övervakade automatiserar uppgraderingen och hälsokontrollen för programmet.</span><span class="sxs-lookup"><span data-stu-id="0718b-116">A Monitored upgrade automates the upgrade and application health check.</span></span>
* <span data-ttu-id="0718b-117">Uppgradering UnmonitoredAuto automatiserar uppgraderingen, men hoppar över hälsokontrollen för programmet.</span><span class="sxs-lookup"><span data-stu-id="0718b-117">An UnmonitoredAuto upgrade automates the upgrade, but skips the application health check.</span></span>
* <span data-ttu-id="0718b-118">När du gör en uppgradering UnmonitoredManual som du behöver uppgradera manuellt varje domän.</span><span class="sxs-lookup"><span data-stu-id="0718b-118">When you do an UnmonitoredManual upgrade, you need to manually upgrade each upgrade domain.</span></span>

<span data-ttu-id="0718b-119">Varje Uppgraderingsläge kräver olika uppsättningar med parametrar.</span><span class="sxs-lookup"><span data-stu-id="0718b-119">Each upgrade mode requires different sets of parameters.</span></span> <span data-ttu-id="0718b-120">Se [uppgradera applikationsparametrarna](service-fabric-application-upgrade-parameters.md) vill veta mer om alternativen för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="0718b-120">See [Application upgrade parameters](service-fabric-application-upgrade-parameters.md) to learn more about the available upgrade options.</span></span>

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="0718b-121">Uppgradera ett Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0718b-121">Upgrade a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="0718b-122">Om du använder Visual Studio Service Fabric-verktyg för att uppgradera ett Service Fabric-program kan du ange en Publicera process för att vara en uppgradering i stället för en vanlig distribution genom att kontrollera den **uppgradera programmet** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="0718b-122">If you’re using the Visual Studio Service Fabric tools to upgrade a Service Fabric application, you can specify a publish process to be an upgrade rather than a regular deployment by checking the **Upgrade the application** check box.</span></span>

### <a name="to-configure-the-upgrade-parameters"></a><span data-ttu-id="0718b-123">Att konfigurera parametrar för uppgradering</span><span class="sxs-lookup"><span data-stu-id="0718b-123">To configure the upgrade parameters</span></span>
1. <span data-ttu-id="0718b-124">Klicka på den **inställningar** knappen bredvid kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="0718b-124">Click the **Settings** button next to the check box.</span></span> <span data-ttu-id="0718b-125">Den **redigera uppgradera parametrar** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="0718b-125">The **Edit Upgrade Parameters** dialog box appears.</span></span> <span data-ttu-id="0718b-126">Den **redigera uppgradera parametrar** dialogrutan stöder uppgradering lägen övervakade, UnmonitoredAuto och UnmonitoredManual.</span><span class="sxs-lookup"><span data-stu-id="0718b-126">The **Edit Upgrade Parameters** dialog box supports the Monitored, UnmonitoredAuto, and UnmonitoredManual upgrade modes.</span></span>
2. <span data-ttu-id="0718b-127">Välj Uppgraderingsläge som du vill använda och sedan fylla i parametern rutnätet.</span><span class="sxs-lookup"><span data-stu-id="0718b-127">Select the upgrade mode that you want to use and then fill out the parameter grid.</span></span>

    <span data-ttu-id="0718b-128">Varje parameter har standardvärden.</span><span class="sxs-lookup"><span data-stu-id="0718b-128">Each parameter has default values.</span></span> <span data-ttu-id="0718b-129">Den valfria parametern *DefaultServiceTypeHealthPolicy* tar en hash-tabell inmatning.</span><span class="sxs-lookup"><span data-stu-id="0718b-129">The optional parameter *DefaultServiceTypeHealthPolicy* takes a hash table input.</span></span> <span data-ttu-id="0718b-130">Här är ett exempel på hash-tabell Indataformatet för *DefaultServiceTypeHealthPolicy*:</span><span class="sxs-lookup"><span data-stu-id="0718b-130">Here’s an example of the hash table input format for *DefaultServiceTypeHealthPolicy*:</span></span>

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    <span data-ttu-id="0718b-131">*ServiceTypeHealthPolicyMap* är en annan valfri parameter som tar en hash-tabell indata i följande format:</span><span class="sxs-lookup"><span data-stu-id="0718b-131">*ServiceTypeHealthPolicyMap* is another optional parameter that takes a hash table input in the following format:</span></span>

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    <span data-ttu-id="0718b-132">Här är en verklig exempel:</span><span class="sxs-lookup"><span data-stu-id="0718b-132">Here's a real-life example:</span></span>

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. <span data-ttu-id="0718b-133">Om du väljer UnmonitoredManual Uppgraderingsläge, måste du starta en PowerShell-konsol för att fortsätta och Slutför uppgraderingen manuellt.</span><span class="sxs-lookup"><span data-stu-id="0718b-133">If you select UnmonitoredManual upgrade mode, you must manually start a PowerShell console to continue and finish the upgrade process.</span></span> <span data-ttu-id="0718b-134">Referera till [uppgradering av Service Fabric-programmet: avancerade alternativ](service-fabric-application-upgrade-advanced.md) att lära dig hur manuell uppgradering fungerar.</span><span class="sxs-lookup"><span data-stu-id="0718b-134">Refer to [Service Fabric application upgrade: advanced topics](service-fabric-application-upgrade-advanced.md) to learn how manual upgrade works.</span></span>

## <a name="upgrade-an-application-by-using-powershell"></a><span data-ttu-id="0718b-135">Uppgradera ett program med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="0718b-135">Upgrade an application by using PowerShell</span></span>
<span data-ttu-id="0718b-136">Du kan använda PowerShell-cmdlets för att uppgradera ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="0718b-136">You can use PowerShell cmdlets to upgrade a Service Fabric application.</span></span> <span data-ttu-id="0718b-137">Se [Service Fabric uppgradera självstudien](service-fabric-application-upgrade-tutorial.md) och [Start ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="0718b-137">See [Service Fabric application upgrade tutorial](service-fabric-application-upgrade-tutorial.md) and [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) for detailed information.</span></span>

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a><span data-ttu-id="0718b-138">Ange en hälsoprincip för kontrollen i programmanifestfilen</span><span class="sxs-lookup"><span data-stu-id="0718b-138">Specify a health check policy in the application manifest file</span></span>
<span data-ttu-id="0718b-139">Varje tjänst i ett Service Fabric-program kan ha sin egen parametrar för hälsotillstånd som åsidosätter standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="0718b-139">Every service in a Service Fabric application can have its own health policy parameters that override the default values.</span></span> <span data-ttu-id="0718b-140">Du kan ange dessa parametervärden i programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="0718b-140">You can provide these parameter values in the application manifest file.</span></span>

<span data-ttu-id="0718b-141">I följande exempel visar hur du kan tillämpa en princip för kontroll av unikt hälsotillstånd för varje tjänst i programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="0718b-141">The following example shows how to apply a unique health check policy for each service in the application manifest.</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="0718b-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0718b-142">Next steps</span></span>
<span data-ttu-id="0718b-143">Mer information om hur du distribuerar ett program finns [distribuera ett befintligt program i Azure Service Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="0718b-143">For more information about deploying an application, see [Deploy an existing application in Azure Service Fabric](service-fabric-deploy-existing-app.md).</span></span>