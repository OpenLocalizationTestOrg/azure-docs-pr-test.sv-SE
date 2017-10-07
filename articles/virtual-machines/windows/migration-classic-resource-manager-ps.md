---
title: aaaMigrate tooResource Manager med PowerShell | Microsoft Docs
description: "Den här artikeln innehåller stegvisa hello stöds av plattformen migrering av IaaS-resurser som virtuella datorer (VM), virtuella nätverk (Vnet) och storage-konton från klassiska tooAzure Resource Manager (ARM) med hjälp av Azure PowerShell-kommandon"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: b5b2987da292f1c241be71a354b0c2e1a96a07c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-powershell"></a><span data-ttu-id="1f95f-103">Migrera IaaS-resurser från klassiska tooAzure Resource Manager med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f95f-103">Migrate IaaS resources from classic tooAzure Resource Manager by using Azure PowerShell</span></span>
<span data-ttu-id="1f95f-104">Dessa steg visar hur toouse Azure PowerShell-kommandon toomigrate infrastruktur som en tjänst (IaaS)-resurser från hello klassisk distribution modellen toohello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-104">These steps show you how toouse Azure PowerShell commands toomigrate infrastructure as a service (IaaS) resources from hello classic deployment model toohello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="1f95f-105">Om du vill kan du migrera resurser med hjälp av hello [Azure-kommandoradsgränssnittet (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1f95f-105">If you want, you can also migrate resources by using hello [Azure Command Line Interface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span></span>

* <span data-ttu-id="1f95f-106">Bakgrundsinformation om migreringsscenarier som stöds finns i [plattform som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1f95f-106">For background on supported migration scenarios, see [Platform-supported migration of IaaS resources from classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md).</span></span>
* <span data-ttu-id="1f95f-107">Detaljerad information och en genomgång för migrering finns i [tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span><span class="sxs-lookup"><span data-stu-id="1f95f-107">For detailed guidance and a migration walkthrough, see [Technical deep dive on platform-supported migration from classic tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span></span>
* [<span data-ttu-id="1f95f-108">Granska de vanligaste migreringsfelen</span><span class="sxs-lookup"><span data-stu-id="1f95f-108">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md)

<br>
<span data-ttu-id="1f95f-109">Här är en flödesschema tooidentify hello ordning där steg måste toobe körs under en migreringsprocessen</span><span class="sxs-lookup"><span data-stu-id="1f95f-109">Here is a flowchart tooidentify hello order in which steps need toobe executed during a migration process</span></span>

![Skärmbild som visar hello migreringssteg](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a><span data-ttu-id="1f95f-111">Steg 1: Planera för migrering</span><span class="sxs-lookup"><span data-stu-id="1f95f-111">Step 1: Plan for migration</span></span>
<span data-ttu-id="1f95f-112">Här följer några metodtips som vi rekommenderar medan du utvärderar migrera IaaS-resurser från klassiska tooResource Manager:</span><span class="sxs-lookup"><span data-stu-id="1f95f-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic tooResource Manager:</span></span>

* <span data-ttu-id="1f95f-113">Läs igenom hello [stöds inte funktioner och konfigurationer som stöds och](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1f95f-113">Read through hello [supported and unsupported features and configurations](migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="1f95f-114">Om du har virtuella datorer som använder icke-stödda konfigurationer eller funktioner, rekommenderar vi att du väntar hello configuration-funktionen stöd toobe meddelats.</span><span class="sxs-lookup"><span data-stu-id="1f95f-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for hello configuration/feature support toobe announced.</span></span> <span data-ttu-id="1f95f-115">Du kan också den passar dina behov, ta bort funktionen eller flytta från den tooenable konfigurationsmigreringen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-115">Alternatively, if it suits your needs, remove that feature or move out of that configuration tooenable migration.</span></span>
* <span data-ttu-id="1f95f-116">Om du har automatiserat skript som distribuerar din infrastruktur och dina program idag, försök toocreate en liknande inställningar med hjälp av dessa skript för migrering.</span><span class="sxs-lookup"><span data-stu-id="1f95f-116">If you have automated scripts that deploy your infrastructure and applications today, try toocreate a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="1f95f-117">Du kan också ställa in exempel miljöer med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-117">Alternatively, you can set up sample environments by using hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1f95f-118">Programgatewayer stöds inte för migrering från klassiska tooResource Manager.</span><span class="sxs-lookup"><span data-stu-id="1f95f-118">Application Gateways are not currently supported for migration from classic tooResource Manager.</span></span> <span data-ttu-id="1f95f-119">toomigrate ett klassiskt virtuellt nätverk med en Programgateway ta bort hello gateway innan du kör ett förbereda åtgärden toomove hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="1f95f-119">toomigrate a classic virtual network with an Application gateway, remove hello gateway before running a Prepare operation toomove hello network.</span></span> <span data-ttu-id="1f95f-120">När du har slutfört migreringen hello återansluta hello gateway i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1f95f-120">After you complete hello migration, reconnect hello gateway in Azure Resource Manager.</span></span>
>
><span data-ttu-id="1f95f-121">ExpressRoute-gatewayer som ansluter tooExpressRoute kretsar i en annan prenumeration som inte kan migreras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1f95f-121">ExpressRoute gateways connecting tooExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="1f95f-122">I sådana fall kan ta bort hello ExpressRoute-gateway, migrera hello virtuella nätverk och återskapa hello gateway.</span><span class="sxs-lookup"><span data-stu-id="1f95f-122">In such cases, remove hello ExpressRoute gateway, migrate hello virtual network and recreate hello gateway.</span></span> <span data-ttu-id="1f95f-123">Se [migrera ExpressRoute-kretsar och associerade virtuella nätverk från hello klassiska toohello Resource Manager-distributionsmodellen](../../expressroute/expressroute-migration-classic-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1f95f-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from hello classic toohello Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
>
>

## <a name="step-2-install-hello-latest-version-of-azure-powershell"></a><span data-ttu-id="1f95f-124">Steg 2: Installera hello senaste versionen av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f95f-124">Step 2: Install hello latest version of Azure PowerShell</span></span>
<span data-ttu-id="1f95f-125">Det finns två huvudsakliga alternativ tooinstall Azure PowerShell: [PowerShell-galleriet](https://www.powershellgallery.com/profiles/azure-sdk/) eller [installationsprogram för webbplattform (WebPI)](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="1f95f-125">There are two main options tooinstall Azure PowerShell: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) or [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="1f95f-126">WebPI tar emot uppdateringar varje månad.</span><span class="sxs-lookup"><span data-stu-id="1f95f-126">WebPI receives monthly updates.</span></span> <span data-ttu-id="1f95f-127">PowerShell-galleriet tar emot uppdateringar kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="1f95f-127">PowerShell Gallery receives updates on a continuous basis.</span></span> <span data-ttu-id="1f95f-128">Den här artikeln är baserad på Azure PowerShell version 2.1.0.</span><span class="sxs-lookup"><span data-stu-id="1f95f-128">This article is based on Azure PowerShell version 2.1.0.</span></span>

<span data-ttu-id="1f95f-129">Installationsanvisningar finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1f95f-129">For installation instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-hello-subscription-in-azure-portal"></a><span data-ttu-id="1f95f-130">Steg 3: Kontrollera att du är administratör för hello prenumeration i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1f95f-130">Step 3: Ensure that you are an administrator for hello subscription in Azure portal</span></span>
<span data-ttu-id="1f95f-131">tooperform migreringen, du måste läggas till som medadministratör för prenumerationen hello i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1f95f-131">tooperform this migration, you must be added as a co-administrator for hello subscription in hello [Azure portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="1f95f-132">Logga in på hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1f95f-132">Sign into hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1f95f-133">På navmenyn hello väljer **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-133">On hello Hub menu, select **Subscription**.</span></span> <span data-ttu-id="1f95f-134">Om du inte ser det, väljer **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-134">If you don't see it, select **More services**.</span></span>
3. <span data-ttu-id="1f95f-135">Hitta hello lämpliga prenumerationspost titta sedan på hello **Mina ROLLEN** fältet.</span><span class="sxs-lookup"><span data-stu-id="1f95f-135">Find hello appropriate subscription entry, then look at hello **MY ROLE** field.</span></span> <span data-ttu-id="1f95f-136">För en medadministratör hello-värdet bör vara _kontoadministratören_.</span><span class="sxs-lookup"><span data-stu-id="1f95f-136">For a co-administrator, hello value should be _Account admin_.</span></span>

<span data-ttu-id="1f95f-137">Om du inte kan tooadd en medadministratör Kontakta sedan en tjänstadministratör eller en medadministratör för hello prenumeration tooget själv lagts till.</span><span class="sxs-lookup"><span data-stu-id="1f95f-137">If you are not able tooadd a co-administrator, then contact a service administrator or co-administrator for hello subscription tooget yourself added.</span></span>   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a><span data-ttu-id="1f95f-138">Steg 4: Ange din prenumeration och registrera dig för migrering</span><span class="sxs-lookup"><span data-stu-id="1f95f-138">Step 4: Set your subscription and sign up for migration</span></span>
<span data-ttu-id="1f95f-139">Först, starta PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="1f95f-139">First, start a PowerShell prompt.</span></span> <span data-ttu-id="1f95f-140">För migrering, behöver du tooset upp din miljö för både klassiska och Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1f95f-140">For migration, you need tooset up your environment for both classic and Resource Manager.</span></span>

<span data-ttu-id="1f95f-141">Logga in tooyour konto för hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-141">Sign in tooyour account for hello Resource Manager model.</span></span>

```powershell
    Login-AzureRmAccount
```

<span data-ttu-id="1f95f-142">Hämta hello tillgängliga prenumerationer med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-142">Get hello available subscriptions by using hello following command:</span></span>

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

<span data-ttu-id="1f95f-143">Ange din Azure-prenumeration för hello aktuell session.</span><span class="sxs-lookup"><span data-stu-id="1f95f-143">Set your Azure subscription for hello current session.</span></span> <span data-ttu-id="1f95f-144">Det här exemplet anger hello prenumeration för standardnamnet för**min Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-144">This example sets hello default subscription name too**My Azure Subscription**.</span></span> <span data-ttu-id="1f95f-145">Ersätt hello exempel prenumerationsnamn med din egen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-145">Replace hello example subscription name with your own.</span></span>

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> <span data-ttu-id="1f95f-146">Registreringen är en enstaka steg, men du måste göra detta en gång innan du försöker migrera.</span><span class="sxs-lookup"><span data-stu-id="1f95f-146">Registration is a one-time step, but you must do it once before attempting migration.</span></span> <span data-ttu-id="1f95f-147">Utan registrering, kan du se hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="1f95f-147">Without registering, you see hello following error message:</span></span>
>
> <span data-ttu-id="1f95f-148">*BadRequest: Prenumerationen har inte registrerats för migrering.*</span><span class="sxs-lookup"><span data-stu-id="1f95f-148">*BadRequest : Subscription is not registered for migration.*</span></span>
>
>

<span data-ttu-id="1f95f-149">Registrera med resursprovidern för hello migrering med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-149">Register with hello migration resource provider by using hello following command:</span></span>

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="1f95f-150">Vänta 5 minuter för hello registrering toofinish.</span><span class="sxs-lookup"><span data-stu-id="1f95f-150">Please wait five minutes for hello registration toofinish.</span></span> <span data-ttu-id="1f95f-151">Du kan kontrollera hello status för hello godkännandet med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-151">You can check hello status of hello approval by using hello following command:</span></span>

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="1f95f-152">Se till att RegistrationState `Registered` innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="1f95f-152">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

<span data-ttu-id="1f95f-153">Nu logga in tooyour konto för hello klassiska modellen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-153">Now sign in tooyour account for hello classic model.</span></span>

```powershell
    Add-AzureAccount
```

<span data-ttu-id="1f95f-154">Hämta hello tillgängliga prenumerationer med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-154">Get hello available subscriptions by using hello following command:</span></span>

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

<span data-ttu-id="1f95f-155">Ange din Azure-prenumeration för hello aktuell session.</span><span class="sxs-lookup"><span data-stu-id="1f95f-155">Set your Azure subscription for hello current session.</span></span> <span data-ttu-id="1f95f-156">Det här exemplet anger hello standardabonnemang för**min Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-156">This example sets hello default subscription too**My Azure Subscription**.</span></span> <span data-ttu-id="1f95f-157">Ersätt hello exempel prenumerationsnamn med din egen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-157">Replace hello example subscription name with your own.</span></span>

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="1f95f-158">Steg 5: Kontrollera att du har tillräckligt med Azure Resource Manager-dator kärnor i hello Azure-regionen för din aktuella distributionen eller virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="1f95f-158">Step 5: Make sure you have enough Azure Resource Manager Virtual Machine cores in hello Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="1f95f-159">Du kan använda hello följande PowerShell-kommandot toocheck hello aktuellt antal kärnor som du har i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1f95f-159">You can use hello following PowerShell command toocheck hello current number of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="1f95f-160">toolearn mer om grundläggande kvoter finns [gränser och hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span><span class="sxs-lookup"><span data-stu-id="1f95f-160">toolearn more about core quotas, see [Limits and hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span></span>

<span data-ttu-id="1f95f-161">Det här exemplet kontrollerar hello tillgänglighet i hello **västra USA** region.</span><span class="sxs-lookup"><span data-stu-id="1f95f-161">This example checks hello availability in hello **West US** region.</span></span> <span data-ttu-id="1f95f-162">Ersätt hello exempel regionnamn med din egen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-162">Replace hello example region name with your own.</span></span>

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-toomigrate-your-iaas-resources"></a><span data-ttu-id="1f95f-163">Steg 6: Kör kommandon toomigrate IaaS-resurser</span><span class="sxs-lookup"><span data-stu-id="1f95f-163">Step 6: Run commands toomigrate your IaaS resources</span></span>
> [!NOTE]
> <span data-ttu-id="1f95f-164">Alla hello-åtgärder som beskrivs här är idempotent.</span><span class="sxs-lookup"><span data-stu-id="1f95f-164">All hello operations described here are idempotent.</span></span> <span data-ttu-id="1f95f-165">Om du har problem med än en funktion som inte stöds eller ett konfigurationsfel rekommenderar vi att du gör hello förbereda, avbrytas eller genomföras igen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-165">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry hello prepare, abort, or commit operation.</span></span> <span data-ttu-id="1f95f-166">hello plattform försök görs hello åtgärden igen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-166">hello platform then tries hello action again.</span></span>
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a><span data-ttu-id="1f95f-167">Steg 6.1: Alternativ 1 – migrera virtuella datorer i en tjänst i molnet (inte i ett virtuellt nätverk)</span><span class="sxs-lookup"><span data-stu-id="1f95f-167">Step 6.1: Option 1 - Migrate virtual machines in a cloud service (not in a virtual network)</span></span>
<span data-ttu-id="1f95f-168">Hämta hello lista över molntjänster med hjälp av följande kommando hello och välj hello molnbaserad tjänst som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="1f95f-168">Get hello list of cloud services by using hello following command, and then pick hello cloud service that you want toomigrate.</span></span> <span data-ttu-id="1f95f-169">Om hello virtuella datorer i hello Molntjänsten i ett virtuellt nätverk eller om de har webb-eller arbetsroller returnerar hello kommando ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1f95f-169">If hello VMs in hello cloud service are in a virtual network or if they have web or worker roles, hello command returns an error message.</span></span>

```powershell
    Get-AzureService | ft Servicename
```

<span data-ttu-id="1f95f-170">Hämta hello distributionens namn för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="1f95f-170">Get hello deployment name for hello cloud service.</span></span> <span data-ttu-id="1f95f-171">I det här exemplet hello tjänstnamnet är **Mina tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-171">In this example, hello service name is **My Service**.</span></span> <span data-ttu-id="1f95f-172">Ersätt hello exempel tjänstnamn med egna namn.</span><span class="sxs-lookup"><span data-stu-id="1f95f-172">Replace hello example service name with your own service name.</span></span>

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

<span data-ttu-id="1f95f-173">Förbered hello virtuella datorer i hello Molntjänsten för migrering.</span><span class="sxs-lookup"><span data-stu-id="1f95f-173">Prepare hello virtual machines in hello cloud service for migration.</span></span> <span data-ttu-id="1f95f-174">Du har två alternativ toochoose från.</span><span class="sxs-lookup"><span data-stu-id="1f95f-174">You have two options toochoose from.</span></span>

* <span data-ttu-id="1f95f-175">**Alternativ 1. Migrera hello VMs tooa plattform skapa virtuellt nätverk**</span><span class="sxs-lookup"><span data-stu-id="1f95f-175">**Option 1. Migrate hello VMs tooa platform-created virtual network**</span></span>

    <span data-ttu-id="1f95f-176">Verifiera först om du kan migrera hello Molntjänsten med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="1f95f-176">First, validate if you can migrate hello cloud service using hello following commands:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    <span data-ttu-id="1f95f-177">hello Visar föregående kommando alla varningar och fel som blockerar migrering.</span><span class="sxs-lookup"><span data-stu-id="1f95f-177">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="1f95f-178">Om verifieringen lyckas, så du kan flytta på toohello **Förbered** steg:</span><span class="sxs-lookup"><span data-stu-id="1f95f-178">If validation is successful, then you can move on toohello **Prepare** step:</span></span>

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* <span data-ttu-id="1f95f-179">**Alternativ 2. Migrera tooan befintligt virtuellt nätverk i hello Resource Manager-distributionsmodellen**</span><span class="sxs-lookup"><span data-stu-id="1f95f-179">**Option 2. Migrate tooan existing virtual network in hello Resource Manager deployment model**</span></span>

    <span data-ttu-id="1f95f-180">Det här exemplet anger hello resursgruppens namn för**myResourceGroup**, hello virtuella nätverksnamnet för**myVirtualNetwork** och hello undernätsnamn för**mySubNet**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-180">This example sets hello resource group name too**myResourceGroup**, hello virtual network name too**myVirtualNetwork** and hello subnet name too**mySubNet**.</span></span> <span data-ttu-id="1f95f-181">Ersätt hello namn i hello exemplet med hello namnen på de egna resurser.</span><span class="sxs-lookup"><span data-stu-id="1f95f-181">Replace hello names in hello example with hello names of your own resources.</span></span>

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    <span data-ttu-id="1f95f-182">Verifiera först om du kan migrera hello virtuellt nätverk med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-182">First, validate if you can migrate hello virtual network using hello following command:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    <span data-ttu-id="1f95f-183">hello Visar föregående kommando alla varningar och fel som blockerar migrering.</span><span class="sxs-lookup"><span data-stu-id="1f95f-183">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="1f95f-184">Om verifieringen lyckas, kan du fortsätta med hello följande förberedelsesteget:</span><span class="sxs-lookup"><span data-stu-id="1f95f-184">If validation is successful, then you can proceed with hello following Prepare step:</span></span>

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

<span data-ttu-id="1f95f-185">När hello Förberedelseåtgärden lyckas med något av föregående alternativ hello, fråga hello migrering tillstånd hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1f95f-185">After hello Prepare operation succeeds with either of hello preceding options, query hello migration state of hello VMs.</span></span> <span data-ttu-id="1f95f-186">Se till att de är i hello `Prepared` tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1f95f-186">Ensure that they are in hello `Prepared` state.</span></span>

<span data-ttu-id="1f95f-187">Det här exemplet anger hello namn på virtuell dator för**myVM**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-187">This example sets hello VM name too**myVM**.</span></span> <span data-ttu-id="1f95f-188">Ersätt hello Exempelnamn med ditt eget namn på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1f95f-188">Replace hello example name with your own VM name.</span></span>

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

<span data-ttu-id="1f95f-189">Kontrollera hello-konfigurationen för hello förberedd resurser med hjälp av PowerShell eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-189">Check hello configuration for hello prepared resources by using either PowerShell or hello Azure portal.</span></span> <span data-ttu-id="1f95f-190">Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-190">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

<span data-ttu-id="1f95f-191">Om hello förberedda konfigurationen ser bra ut du gå vidare och genomför hello resurser med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-191">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="1f95f-192">Steg 6.1: Alternativ 2 – migrera virtuella datorer i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1f95f-192">Step 6.1: Option 2 - Migrate virtual machines in a virtual network</span></span>

<span data-ttu-id="1f95f-193">toomigrate virtuella datorer i ett virtuellt nätverk som du migrerar hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1f95f-193">toomigrate virtual machines in a virtual network, you migrate hello virtual network.</span></span> <span data-ttu-id="1f95f-194">att migrera automatiskt hello virtuella datorer med hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1f95f-194">hello virtual machines automatically migrate with hello virtual network.</span></span> <span data-ttu-id="1f95f-195">Välj hello virtuella nätverk som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="1f95f-195">Pick hello virtual network that you want toomigrate.</span></span>
> [!NOTE]
> <span data-ttu-id="1f95f-196">[Migrera klassiska virtuell dator](migrate-single-classic-to-resource-manager.md) genom att skapa en ny Resource Manager virtuell dator med hanterade diskar med hjälp av hello VHD-(OS och data) filer för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1f95f-196">[Migrate single classic virtual machine](migrate-single-classic-to-resource-manager.md) by creating a new Resource Manager virtual machine with Managed Disks using hello VHD (OS and data) files of hello virtual machine.</span></span>
<br>

> [!NOTE]
> <span data-ttu-id="1f95f-197">hello virtuella nätverksnamnet kan skilja sig från vad som anges i hello nya portalen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-197">hello virtual network name might be different from what is shown in hello new Portal.</span></span> <span data-ttu-id="1f95f-198">hello nya Azure-portalen visar hello namn som `[vnet-name]` men hello faktiska virtuella nätverksnamnet är av typen `Group [resource-group-name] [vnet-name]`.</span><span class="sxs-lookup"><span data-stu-id="1f95f-198">hello new Azure Portal displays hello name as `[vnet-name]` but hello actual virtual network name is of type `Group [resource-group-name] [vnet-name]`.</span></span> <span data-ttu-id="1f95f-199">Innan du migrerar sökning hello faktiska virtuella nätverksnamnet hello kommandot `Get-AzureVnetSite | Select -Property Name` eller hello gamla Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-199">Before migrating, lookup hello actual virtual network name using hello command `Get-AzureVnetSite | Select -Property Name` or view it in hello old Azure Portal.</span></span> 

<span data-ttu-id="1f95f-200">Det här exemplet anger hello virtuella nätverksnamnet för**myVnet**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-200">This example sets hello virtual network name too**myVnet**.</span></span> <span data-ttu-id="1f95f-201">Ersätt hello Exempelnamn för virtuellt nätverk med din egen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-201">Replace hello example virtual network name with your own.</span></span>

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> <span data-ttu-id="1f95f-202">Om hello virtuellt nätverk innehåller webb- eller arbetsroller eller virtuella datorer med konfigurationer som inte stöds, kan du få ett felmeddelande för verifiering.</span><span class="sxs-lookup"><span data-stu-id="1f95f-202">If hello virtual network contains web or worker roles, or VMs with unsupported configurations, you get a validation error message.</span></span>
>
>

<span data-ttu-id="1f95f-203">Verifiera först om du kan migrera hello virtuellt nätverk med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-203">First, validate if you can migrate hello virtual network by using hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

<span data-ttu-id="1f95f-204">hello Visar föregående kommando alla varningar och fel som blockerar migrering.</span><span class="sxs-lookup"><span data-stu-id="1f95f-204">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="1f95f-205">Om verifieringen lyckas, kan du fortsätta med hello följande förberedelsesteget:</span><span class="sxs-lookup"><span data-stu-id="1f95f-205">If validation is successful, then you can proceed with hello following Prepare step:</span></span>

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

<span data-ttu-id="1f95f-206">Kontrollera hello-konfigurationen för hello förbereda virtuella datorer med hjälp av Azure PowerShell eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-206">Check hello configuration for hello prepared virtual machines by using either Azure PowerShell or hello Azure portal.</span></span> <span data-ttu-id="1f95f-207">Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-207">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

<span data-ttu-id="1f95f-208">Om hello förberedda konfigurationen ser bra ut du gå vidare och genomför hello resurser med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-208">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a><span data-ttu-id="1f95f-209">Steg 6.2 Migrera ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="1f95f-209">Step 6.2 Migrate a storage account</span></span>
<span data-ttu-id="1f95f-210">När du är klar migrera hello virtuella datorer, rekommenderar vi att du migrerar hello storage-konton.</span><span class="sxs-lookup"><span data-stu-id="1f95f-210">Once you're done migrating hello virtual machines, we recommend you migrate hello storage accounts.</span></span>

<span data-ttu-id="1f95f-211">Utför föregående nödvändiga kontroller innan du migrerar hello storage-konto:</span><span class="sxs-lookup"><span data-stu-id="1f95f-211">Before you migrate hello storage account, please perform preceding prerequisite checks:</span></span>

* <span data-ttu-id="1f95f-212">**Migrera den klassiska virtuella datorer vars diskar lagras i hello storage-konto**</span><span class="sxs-lookup"><span data-stu-id="1f95f-212">**Migrate classic virtual machines whose disks are stored in hello storage account**</span></span>

    <span data-ttu-id="1f95f-213">Föregående kommando returnerar RoleName och DiskName egenskaper för alla hello klassiska Virtuella diskar i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1f95f-213">Preceding command returns RoleName and DiskName properties of all hello classic VM disks in hello storage account.</span></span> <span data-ttu-id="1f95f-214">RoleName är hello namn hello virtuella toowhich en disk som är ansluten.</span><span class="sxs-lookup"><span data-stu-id="1f95f-214">RoleName is hello name of hello virtual machine toowhich a disk is attached.</span></span> <span data-ttu-id="1f95f-215">Om föregående kommando returnerar diskar Se till att virtuella datorer toowhich diskarna är anslutna migreras innan du migrerar hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1f95f-215">If preceding command returns disks then ensure that virtual machines toowhich these disks are attached are migrated before migrating hello storage account.</span></span>
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* <span data-ttu-id="1f95f-216">**Ta bort beräkningsfunktionen klassiska Virtuella diskar som lagras i hello storage-konto**</span><span class="sxs-lookup"><span data-stu-id="1f95f-216">**Delete unattached classic VM disks stored in hello storage account**</span></span>

    <span data-ttu-id="1f95f-217">Hitta beräkningsfunktionen klassiska Virtuella diskar i hello storage-konto med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-217">Find unattached classic VM disks in hello storage account using following command:</span></span>

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    <span data-ttu-id="1f95f-218">Om ovanstående kommando returnerar diskar sedan ta bort dessa diskar med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-218">If above command returns disks then delete these disks using following command:</span></span>

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* <span data-ttu-id="1f95f-219">**Ta bort VM-avbildningar lagras i hello storage-konto**</span><span class="sxs-lookup"><span data-stu-id="1f95f-219">**Delete VM images stored in hello storage account**</span></span>

    <span data-ttu-id="1f95f-220">Föregående kommando returnerar alla hello VM-avbildningar med OS-disken som lagras i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1f95f-220">Preceding command returns all hello VM images with OS disk stored in hello storage account.</span></span>
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     <span data-ttu-id="1f95f-221">Föregående kommando returnerar alla hello VM-avbildningar med datadiskar som lagras i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1f95f-221">Preceding command returns all hello VM images with data disks stored in hello storage account.</span></span>
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    <span data-ttu-id="1f95f-222">Ta bort alla hello VM-avbildningar som returneras av ovan kommandon med hjälp av föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-222">Delete all hello VM images returned by above commands using preceding command:</span></span>
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

<span data-ttu-id="1f95f-223">Verifiera varje lagringskonto för migrering med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="1f95f-223">Validate each storage account for migration by using hello following command.</span></span> <span data-ttu-id="1f95f-224">I det här exemplet hello lagringskontonamnet är **Mittlagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-224">In this example, hello storage account name is **myStorageAccount**.</span></span> <span data-ttu-id="1f95f-225">Ersätt hello Exempelnamn med hello namnet på ditt eget lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="1f95f-225">Replace hello example name with hello name of your own storage account.</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

<span data-ttu-id="1f95f-226">Nästa steg är tooPrepare hello lagringskonto för migrering</span><span class="sxs-lookup"><span data-stu-id="1f95f-226">Next step is tooPrepare hello storage account for migration</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

<span data-ttu-id="1f95f-227">Kontrollera hello-konfigurationen för hello förberedd storage-konto med hjälp av Azure PowerShell eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1f95f-227">Check hello configuration for hello prepared storage account by using either Azure PowerShell or hello Azure portal.</span></span> <span data-ttu-id="1f95f-228">Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-228">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

<span data-ttu-id="1f95f-229">Om hello förberedda konfigurationen ser bra ut du gå vidare och genomför hello resurser med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1f95f-229">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a><span data-ttu-id="1f95f-230">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f95f-230">Next steps</span></span>
* [<span data-ttu-id="1f95f-231">Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1f95f-231">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="1f95f-232">Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1f95f-232">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="1f95f-233">Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1f95f-233">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="1f95f-234">Använd CLI toomigrate IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1f95f-234">Use CLI toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="1f95f-235">Community-verktyg för att hjälpa till med migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1f95f-235">Community tools for assisting with migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="1f95f-236">Granska de vanligaste migreringsfelen</span><span class="sxs-lookup"><span data-stu-id="1f95f-236">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="1f95f-237">Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1f95f-237">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
