---
title: "Flytta ExpressRoute-kretsar från klassiska tooResource Manager: PowerShell: Azure | Microsoft Docs"
description: "Den här sidan beskrivs hur toomove klassiska kretsen-toohello Resource Manager distribution modellen med hjälp av PowerShell."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8dcadafca5e4f40773902cec5786eba1dbe133eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model-using-powershell"></a><span data-ttu-id="8199d-103">Flytta ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8199d-103">Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model using PowerShell</span></span>

<span data-ttu-id="8199d-104">toouse en ExpressRoute-krets för både klassiska hello och Resource Manager distributionsmodellerna, måste du flytta hello krets toohello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8199d-104">toouse an ExpressRoute circuit for both hello classic and Resource Manager deployment models, you must move hello circuit toohello Resource Manager deployment model.</span></span> <span data-ttu-id="8199d-105">hello hjälper följande avsnitt dig att flytta kretsen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8199d-105">hello following sections help you move your circuit by using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8199d-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8199d-106">Before you begin</span></span>

* <span data-ttu-id="8199d-107">Kontrollera att du har hello senaste versionen av hello Azure PowerShell-moduler (minst version 1.0).</span><span class="sxs-lookup"><span data-stu-id="8199d-107">Verify that you have hello latest version of hello Azure PowerShell modules (at least version 1.0).</span></span> <span data-ttu-id="8199d-108">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8199d-108">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="8199d-109">Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="8199d-109">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="8199d-110">Granska hello information som tillhandahålls under [flytta en ExpressRoute-krets från klassiska tooResource Manager](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="8199d-110">Review hello information that is provided under [Moving an ExpressRoute circuit from classic tooResource Manager](expressroute-move.md).</span></span> <span data-ttu-id="8199d-111">Kontrollera att du förstår hello gränser och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="8199d-111">Make sure that you fully understand hello limits and limitations.</span></span>
* <span data-ttu-id="8199d-112">Kontrollera att hello kretsen är fullt fungerande i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8199d-112">Verify that hello circuit is fully operational in hello classic deployment model.</span></span>
* <span data-ttu-id="8199d-113">Se till att du har en resursgrupp som skapades i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8199d-113">Ensure that you have a resource group that was created in hello Resource Manager deployment model.</span></span>

## <a name="move-an-expressroute-circuit"></a><span data-ttu-id="8199d-114">Flytta en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="8199d-114">Move an ExpressRoute circuit</span></span>

### <a name="step-1-gather-circuit-details-from-hello-classic-deployment-model"></a><span data-ttu-id="8199d-115">Steg 1: Samla in krets information från hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="8199d-115">Step 1: Gather circuit details from hello classic deployment model</span></span>

<span data-ttu-id="8199d-116">Logga in toohello klassiska Azure-miljön och samla in hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="8199d-116">Sign in toohello Azure classic environment and gather hello service key.</span></span>

1. <span data-ttu-id="8199d-117">Logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="8199d-117">Sign in tooyour Azure account.</span></span>

  ```powershell
  Add-AzureAccount
  ```

2. <span data-ttu-id="8199d-118">Välj lämplig Azure hello-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8199d-118">Select hello appropriate Azure subscription.</span></span>

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. <span data-ttu-id="8199d-119">Importera hello PowerShell-moduler för Azure och ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="8199d-119">Import hello PowerShell modules for Azure and ExpressRoute.</span></span>

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. <span data-ttu-id="8199d-120">Använd hello-cmdlet nedan för för tooget hello-tjänsten för alla ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="8199d-120">Use hello cmdlet below tooget hello service keys for all of your ExpressRoute circuits.</span></span> <span data-ttu-id="8199d-121">Efter att du hämtar hello nycklar, kopiera hello **tjänstnyckeln** av hello krets som du vill toomove toohello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8199d-121">After retrieving hello keys, copy hello **service key** of hello circuit that you want toomove toohello Resource Manager deployment model.</span></span>

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a><span data-ttu-id="8199d-122">Steg 2: Logga in och skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="8199d-122">Step 2: Sign in and create a resource group</span></span>

<span data-ttu-id="8199d-123">Logga in toohello Resource Manager-miljön och skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8199d-123">Sign in toohello Resource Manager environment and create a new resource group.</span></span>

1. <span data-ttu-id="8199d-124">Logga in tooyour Azure Resource Manager-miljön.</span><span class="sxs-lookup"><span data-stu-id="8199d-124">Sign in tooyour Azure Resource Manager environment.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="8199d-125">Välj lämplig Azure hello-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8199d-125">Select hello appropriate Azure subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. <span data-ttu-id="8199d-126">Ändra hello fragment nedan toocreate en ny resursgrupp om du inte redan har en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8199d-126">Modify hello snippet below toocreate a new resource group if you don't already have a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-hello-expressroute-circuit-toohello-resource-manager-deployment-model"></a><span data-ttu-id="8199d-127">Steg 3: Flytta hello ExpressRoute-kretsen toohello Resource Manager-modellen</span><span class="sxs-lookup"><span data-stu-id="8199d-127">Step 3: Move hello ExpressRoute circuit toohello Resource Manager deployment model</span></span>

<span data-ttu-id="8199d-128">Du är nu redo toomove ExpressRoute-krets från hello klassisk distribution modellen toohello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8199d-128">You are now ready toomove your ExpressRoute circuit from hello classic deployment model toohello Resource Manager deployment model.</span></span> <span data-ttu-id="8199d-129">Innan du fortsätter bör du granska hello informationen i [flytta en ExpressRoute-krets från hello klassiska toohello Resource Manager-distributionsmodellen](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="8199d-129">Before proceeding, review hello information provided in [Moving an ExpressRoute circuit from hello classic toohello Resource Manager deployment model](expressroute-move.md).</span></span>

<span data-ttu-id="8199d-130">toomove kretsen, ändra och kör följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="8199d-130">toomove your circuit, modify and run hello following snippet:</span></span>

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> <span data-ttu-id="8199d-131">När hello flyttningen är klar kommer hello nya namnet som anges i hello föregående cmdlet att använda tooaddress hello resurs.</span><span class="sxs-lookup"><span data-stu-id="8199d-131">After hello move has finished, hello new name that is listed in hello previous cmdlet will be used tooaddress hello resource.</span></span> <span data-ttu-id="8199d-132">hello krets att i stort sett ändras.</span><span class="sxs-lookup"><span data-stu-id="8199d-132">hello circuit will essentially be renamed.</span></span>
> 

## <a name="modify-circuit-access"></a><span data-ttu-id="8199d-133">Ändra krets åtkomst</span><span class="sxs-lookup"><span data-stu-id="8199d-133">Modify circuit access</span></span>

### <a name="tooenable-expressroute-circuit-access-for-both-deployment-models"></a><span data-ttu-id="8199d-134">tooenable ExpressRoute-kretsen åtkomst för båda distributionsmodellerna</span><span class="sxs-lookup"><span data-stu-id="8199d-134">tooenable ExpressRoute circuit access for both deployment models</span></span>

<span data-ttu-id="8199d-135">När du har flyttat din klassiska distributionsmodellen för ExpressRoute-kretsen toohello Resource Manager kan du aktivera åtkomst tooboth distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="8199d-135">After moving your classic ExpressRoute circuit toohello Resource Manager deployment model, you can enable access tooboth deployment models.</span></span> <span data-ttu-id="8199d-136">Kör följande cmdlets tooenable åtkomst tooboth distributionsmodeller hello:</span><span class="sxs-lookup"><span data-stu-id="8199d-136">Run hello following cmdlets tooenable access tooboth deployment models:</span></span>

1. <span data-ttu-id="8199d-137">Hämta hello krets information.</span><span class="sxs-lookup"><span data-stu-id="8199d-137">Get hello circuit details.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="8199d-138">Ange ”Tillåt klassiska åtgärder” tooTRUE.</span><span class="sxs-lookup"><span data-stu-id="8199d-138">Set "Allow Classic Operations" tooTRUE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. <span data-ttu-id="8199d-139">Uppdatera hello krets.</span><span class="sxs-lookup"><span data-stu-id="8199d-139">Update hello circuit.</span></span> <span data-ttu-id="8199d-140">När den här åtgärden har slutförts, kommer du att kunna tooview hello krets i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8199d-140">After this operation has finished successfully, you will be able tooview hello circuit in hello classic deployment model.</span></span>

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. <span data-ttu-id="8199d-141">Kör följande cmdlet tooget hello information av hello ExpressRoute-krets hello.</span><span class="sxs-lookup"><span data-stu-id="8199d-141">Run hello following cmdlet tooget hello details of hello ExpressRoute circuit.</span></span> <span data-ttu-id="8199d-142">Du måste vara kan toosee hello nyckeln för tjänsten i listan.</span><span class="sxs-lookup"><span data-stu-id="8199d-142">You must be able toosee hello service key listed.</span></span>

  ```powershell
  get-azurededicatedcircuit
  ```

5. <span data-ttu-id="8199d-143">Du kan nu hantera länkar toohello ExpressRoute-krets kommandona hello klassisk distribution modellen för klassiska Vnet och hello Resource Manager-kommandon för Resource Manager VNets.</span><span class="sxs-lookup"><span data-stu-id="8199d-143">You can now manage links toohello ExpressRoute circuit using hello classic deployment model commands for classic VNets, and hello Resource Manager commands for Resource Manager VNets.</span></span> <span data-ttu-id="8199d-144">hello hjälper följande artiklar dig att hantera länkar toohello ExpressRoute-krets:</span><span class="sxs-lookup"><span data-stu-id="8199d-144">hello following articles help you manage links toohello ExpressRoute circuit:</span></span>

    * [<span data-ttu-id="8199d-145">Länka ditt virtuella nätverk tooyour ExpressRoute-krets i hello Resource Manager-distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="8199d-145">Link your virtual network tooyour ExpressRoute circuit in hello Resource Manager deployment model</span></span>](expressroute-howto-linkvnet-arm.md)
    * [<span data-ttu-id="8199d-146">Länka ditt virtuella nätverk tooyour ExpressRoute-krets i hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="8199d-146">Link your virtual network tooyour ExpressRoute circuit in hello classic deployment model</span></span>](expressroute-howto-linkvnet-classic.md)

### <a name="toodisable-expressroute-circuit-access-toohello-classic-deployment-model"></a><span data-ttu-id="8199d-147">toodisable ExpressRoute-kretsen åtkomst toohello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="8199d-147">toodisable ExpressRoute circuit access toohello classic deployment model</span></span>

<span data-ttu-id="8199d-148">Kör följande cmdlets toodisable klassiska distributionsmodellen för åtkomst toohello hello.</span><span class="sxs-lookup"><span data-stu-id="8199d-148">Run hello following cmdlets toodisable access toohello classic deployment model.</span></span>

1. <span data-ttu-id="8199d-149">Hämta information om hello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="8199d-149">Get details of hello ExpressRoute circuit.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="8199d-150">Ange ”Tillåt klassiska åtgärder” tooFALSE.</span><span class="sxs-lookup"><span data-stu-id="8199d-150">Set "Allow Classic Operations" tooFALSE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. <span data-ttu-id="8199d-151">Uppdatera hello krets.</span><span class="sxs-lookup"><span data-stu-id="8199d-151">Update hello circuit.</span></span> <span data-ttu-id="8199d-152">När den här åtgärden har slutförts, kommer inte att kunna tooview hello krets i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8199d-152">After this operation has finished successfully, you will not be able tooview hello circuit in hello classic deployment model.</span></span>

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a><span data-ttu-id="8199d-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8199d-153">Next steps</span></span>

* [<span data-ttu-id="8199d-154">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="8199d-154">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="8199d-155">Länka ditt virtuella nätverk tooyour ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="8199d-155">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)
