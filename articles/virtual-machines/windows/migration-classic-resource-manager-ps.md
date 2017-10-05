---
title: Migrera till Resource Manager med PowerShell | Microsoft Docs
description: "Den här artikeln innehåller stegvisa migreringen plattform som stöds av IaaS-resurser som virtuella datorer (VM), virtuella nätverk (Vnet) och storage-konton från klassiska till Azure Resource Manager (ARM) med hjälp av Azure PowerShell-kommandon"
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
ms.openlocfilehash: 489e6cc6bd3c5b36635f5f7e398d08fed681d2e7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a><span data-ttu-id="cb9b8-103">Migrera IaaS-resurser från klassiska till Azure Resource Manager med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cb9b8-103">Migrate IaaS resources from classic to Azure Resource Manager by using Azure PowerShell</span></span>
<span data-ttu-id="cb9b8-104">Dessa steg visar hur du använder Azure PowerShell-kommandon för att migrera infrastruktur som en tjänst (IaaS)-resurser från den klassiska distributionsmodellen till Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-104">These steps show you how to use Azure PowerShell commands to migrate infrastructure as a service (IaaS) resources from the classic deployment model to the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="cb9b8-105">Om du vill kan du migrera resurser med hjälp av den [Azure-kommandoradsgränssnittet (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-105">If you want, you can also migrate resources by using the [Azure Command Line Interface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span></span>

* <span data-ttu-id="cb9b8-106">Bakgrundsinformation om migreringsscenarier som stöds finns i [plattform som stöds migrering av IaaS-resurser från klassiska till Azure Resource Manager](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-106">For background on supported migration scenarios, see [Platform-supported migration of IaaS resources from classic to Azure Resource Manager](migration-classic-resource-manager-overview.md).</span></span>
* <span data-ttu-id="cb9b8-107">Detaljerad information och en genomgång för migrering finns i [tekniska ingående om plattformen stöds från klassiska till Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-107">For detailed guidance and a migration walkthrough, see [Technical deep dive on platform-supported migration from classic to Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span></span>
* [<span data-ttu-id="cb9b8-108">Granska de vanligaste migreringsfelen</span><span class="sxs-lookup"><span data-stu-id="cb9b8-108">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md)

<br>
<span data-ttu-id="cb9b8-109">Här är ett flödesschema för att identifiera den ordning som steg måste utföras under en migreringsprocessen</span><span class="sxs-lookup"><span data-stu-id="cb9b8-109">Here is a flowchart to identify the order in which steps need to be executed during a migration process</span></span>

![Skärmbild som visar migreringsstegen](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a><span data-ttu-id="cb9b8-111">Steg 1: Planera för migrering</span><span class="sxs-lookup"><span data-stu-id="cb9b8-111">Step 1: Plan for migration</span></span>
<span data-ttu-id="cb9b8-112">Här följer några metodtips som vi rekommenderar medan du utvärderar migrera IaaS-resurser från klassiska till Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic to Resource Manager:</span></span>

* <span data-ttu-id="cb9b8-113">Läs igenom den [stöds inte funktioner och konfigurationer som stöds och](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-113">Read through the [supported and unsupported features and configurations](migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="cb9b8-114">Om du har virtuella datorer som använder icke-stödda konfigurationer eller funktioner, rekommenderar vi att du väntar configuration/funktionsstöd meddelas.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for the configuration/feature support to be announced.</span></span> <span data-ttu-id="cb9b8-115">Du kan också den passar dina behov ta bort funktionen eller flytta från den konfigurationen vill aktivera migrering.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-115">Alternatively, if it suits your needs, remove that feature or move out of that configuration to enable migration.</span></span>
* <span data-ttu-id="cb9b8-116">Om du har automatiserat skript som distribuerar din infrastruktur och dina program idag, försök att skapa en liknande inställningar med hjälp av dessa skript för migrering.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-116">If you have automated scripts that deploy your infrastructure and applications today, try to create a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="cb9b8-117">Du kan också ställa in provet miljöer med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-117">Alternatively, you can set up sample environments by using the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb9b8-118">Programgatewayer stöds inte för migrering från classic till Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-118">Application Gateways are not currently supported for migration from classic to Resource Manager.</span></span> <span data-ttu-id="cb9b8-119">Ta bort gatewayen innan du kör en Förberedelseåtgärden om du vill flytta nätverket för att migrera ett klassiskt virtuellt nätverk med en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-119">To migrate a classic virtual network with an Application gateway, remove the gateway before running a Prepare operation to move the network.</span></span> <span data-ttu-id="cb9b8-120">När du har slutfört migreringen återansluta gateway i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-120">After you complete the migration, reconnect the gateway in Azure Resource Manager.</span></span>
>
><span data-ttu-id="cb9b8-121">ExpressRoute-gatewayer som ansluter till ExpressRoute-kretsar i en annan prenumeration som inte kan migreras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-121">ExpressRoute gateways connecting to ExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="cb9b8-122">I sådana fall kan ta bort ExpressRoute-gatewayen, migrera det virtuella nätverket och skapa gatewayen på nytt.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-122">In such cases, remove the ExpressRoute gateway, migrate the virtual network and recreate the gateway.</span></span> <span data-ttu-id="cb9b8-123">Se [migrera ExpressRoute-kretsar och associerade virtuella nätverk från klassiskt till Resource Manager-distributionsmodellen](../../expressroute/expressroute-migration-classic-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from the classic to the Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
>
>

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a><span data-ttu-id="cb9b8-124">Steg 2: Installera den senaste versionen av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cb9b8-124">Step 2: Install the latest version of Azure PowerShell</span></span>
<span data-ttu-id="cb9b8-125">Det finns två huvudsakliga alternativ för att installera Azure PowerShell: [PowerShell-galleriet](https://www.powershellgallery.com/profiles/azure-sdk/) eller [installationsprogram för webbplattform (WebPI)](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-125">There are two main options to install Azure PowerShell: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) or [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="cb9b8-126">WebPI tar emot uppdateringar varje månad.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-126">WebPI receives monthly updates.</span></span> <span data-ttu-id="cb9b8-127">PowerShell-galleriet tar emot uppdateringar kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-127">PowerShell Gallery receives updates on a continuous basis.</span></span> <span data-ttu-id="cb9b8-128">Den här artikeln är baserad på Azure PowerShell version 2.1.0.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-128">This article is based on Azure PowerShell version 2.1.0.</span></span>

<span data-ttu-id="cb9b8-129">Installationsanvisningar finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-129">For installation instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-the-subscription-in-azure-portal"></a><span data-ttu-id="cb9b8-130">Steg 3: Kontrollera att du är administratör för prenumerationen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cb9b8-130">Step 3: Ensure that you are an administrator for the subscription in Azure portal</span></span>
<span data-ttu-id="cb9b8-131">Om du vill utföra migreringen måste du måste läggas till som medadministratör för prenumerationen på den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-131">To perform this migration, you must be added as a co-administrator for the subscription in the [Azure portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="cb9b8-132">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-132">Sign into the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cb9b8-133">På navmenyn väljer **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-133">On the Hub menu, select **Subscription**.</span></span> <span data-ttu-id="cb9b8-134">Om du inte ser det, väljer **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-134">If you don't see it, select **More services**.</span></span>
3. <span data-ttu-id="cb9b8-135">Hitta lämpliga prenumerationspost titta sedan på den **Mina ROLLEN** fältet.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-135">Find the appropriate subscription entry, then look at the **MY ROLE** field.</span></span> <span data-ttu-id="cb9b8-136">För en medadministratör. värdet bör vara _kontoadministratören_.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-136">For a co-administrator, the value should be _Account admin_.</span></span>

<span data-ttu-id="cb9b8-137">Om du inte kan lägga till en medadministratör kontakta en administratör eller medadministratör för prenumerationen för att hämta själv lagts till.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-137">If you are not able to add a co-administrator, then contact a service administrator or co-administrator for the subscription to get yourself added.</span></span>   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a><span data-ttu-id="cb9b8-138">Steg 4: Ange din prenumeration och registrera dig för migrering</span><span class="sxs-lookup"><span data-stu-id="cb9b8-138">Step 4: Set your subscription and sign up for migration</span></span>
<span data-ttu-id="cb9b8-139">Först, starta PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-139">First, start a PowerShell prompt.</span></span> <span data-ttu-id="cb9b8-140">För migrering, måste du konfigurera din miljö för både klassiska och Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-140">For migration, you need to set up your environment for both classic and Resource Manager.</span></span>

<span data-ttu-id="cb9b8-141">Logga in på ditt konto för Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-141">Sign in to your account for the Resource Manager model.</span></span>

```powershell
    Login-AzureRmAccount
```

<span data-ttu-id="cb9b8-142">Hämta tillgängliga prenumerationer med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-142">Get the available subscriptions by using the following command:</span></span>

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

<span data-ttu-id="cb9b8-143">Ange din Azure-prenumeration för den aktuella sessionen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-143">Set your Azure subscription for the current session.</span></span> <span data-ttu-id="cb9b8-144">Det här exemplet anges prenumeration standardnamnet **min Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-144">This example sets the default subscription name to **My Azure Subscription**.</span></span> <span data-ttu-id="cb9b8-145">Ersätt prenumerationsnamn exempel med din egen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-145">Replace the example subscription name with your own.</span></span>

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> <span data-ttu-id="cb9b8-146">Registreringen är en enstaka steg, men du måste göra detta en gång innan du försöker migrera.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-146">Registration is a one-time step, but you must do it once before attempting migration.</span></span> <span data-ttu-id="cb9b8-147">Utan registrering visas följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-147">Without registering, you see the following error message:</span></span>
>
> <span data-ttu-id="cb9b8-148">*BadRequest: Prenumerationen har inte registrerats för migrering.*</span><span class="sxs-lookup"><span data-stu-id="cb9b8-148">*BadRequest : Subscription is not registered for migration.*</span></span>
>
>

<span data-ttu-id="cb9b8-149">Registrera med resursprovidern migrering med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-149">Register with the migration resource provider by using the following command:</span></span>

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="cb9b8-150">Vänta 5 minuter att slutföra registreringen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-150">Please wait five minutes for the registration to finish.</span></span> <span data-ttu-id="cb9b8-151">Du kan kontrollera status för godkännande med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-151">You can check the status of the approval by using the following command:</span></span>

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="cb9b8-152">Se till att RegistrationState `Registered` innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-152">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

<span data-ttu-id="cb9b8-153">Nu logga in på ditt konto för den klassiska modellen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-153">Now sign in to your account for the classic model.</span></span>

```powershell
    Add-AzureAccount
```

<span data-ttu-id="cb9b8-154">Hämta tillgängliga prenumerationer med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-154">Get the available subscriptions by using the following command:</span></span>

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

<span data-ttu-id="cb9b8-155">Ange din Azure-prenumeration för den aktuella sessionen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-155">Set your Azure subscription for the current session.</span></span> <span data-ttu-id="cb9b8-156">Det här exemplet anger standard-prenumeration till **min Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-156">This example sets the default subscription to **My Azure Subscription**.</span></span> <span data-ttu-id="cb9b8-157">Ersätt prenumerationsnamn exempel med din egen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-157">Replace the example subscription name with your own.</span></span>

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="cb9b8-158">Steg 5: Kontrollera att du har tillräckligt med Azure Resource Manager-dator kärnor i Azure-regionen för din aktuella distributionen eller virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="cb9b8-158">Step 5: Make sure you have enough Azure Resource Manager Virtual Machine cores in the Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="cb9b8-159">Du kan använda följande PowerShell-kommando för att kontrollera det aktuella antalet kärnor som du har i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-159">You can use the following PowerShell command to check the current number of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="cb9b8-160">Mer information om core kvoter finns [gränser och Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span><span class="sxs-lookup"><span data-stu-id="cb9b8-160">To learn more about core quotas, see [Limits and the Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span></span>

<span data-ttu-id="cb9b8-161">Det här exemplet kontrollerar tillgängligheten den **västra USA** region.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-161">This example checks the availability in the **West US** region.</span></span> <span data-ttu-id="cb9b8-162">Ersätt region Exempelnamn med din egen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-162">Replace the example region name with your own.</span></span>

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a><span data-ttu-id="cb9b8-163">Steg 6: Kör kommandon för att migrera IaaS-resurser</span><span class="sxs-lookup"><span data-stu-id="cb9b8-163">Step 6: Run commands to migrate your IaaS resources</span></span>
> [!NOTE]
> <span data-ttu-id="cb9b8-164">Alla åtgärder som beskrivs här är idempotent.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-164">All the operations described here are idempotent.</span></span> <span data-ttu-id="cb9b8-165">Om du har problem med än en funktion som inte stöds eller ett konfigurationsfel rekommenderar vi att du försöka med att förbereda, avbrytas eller genomföras igen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-165">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry the prepare, abort, or commit operation.</span></span> <span data-ttu-id="cb9b8-166">Plattformen försöker sedan igen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-166">The platform then tries the action again.</span></span>
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a><span data-ttu-id="cb9b8-167">Steg 6.1: Alternativ 1 – migrera virtuella datorer i en tjänst i molnet (inte i ett virtuellt nätverk)</span><span class="sxs-lookup"><span data-stu-id="cb9b8-167">Step 6.1: Option 1 - Migrate virtual machines in a cloud service (not in a virtual network)</span></span>
<span data-ttu-id="cb9b8-168">Med följande kommando för att hämta listan över molntjänster och välj sedan den molntjänst som du vill migrera.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-168">Get the list of cloud services by using the following command, and then pick the cloud service that you want to migrate.</span></span> <span data-ttu-id="cb9b8-169">Om de virtuella datorerna i Molntjänsten är i ett virtuellt nätverk eller om de har webb-eller arbetsroller kommandot returnerar ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-169">If the VMs in the cloud service are in a virtual network or if they have web or worker roles, the command returns an error message.</span></span>

```powershell
    Get-AzureService | ft Servicename
```

<span data-ttu-id="cb9b8-170">Hämta distributionens namn för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-170">Get the deployment name for the cloud service.</span></span> <span data-ttu-id="cb9b8-171">I det här exemplet tjänstnamnet är **Mina tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-171">In this example, the service name is **My Service**.</span></span> <span data-ttu-id="cb9b8-172">Ersätt tjänstnamnet exempel med egna namn.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-172">Replace the example service name with your own service name.</span></span>

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

<span data-ttu-id="cb9b8-173">Förbered de virtuella datorerna i Molntjänsten för migrering.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-173">Prepare the virtual machines in the cloud service for migration.</span></span> <span data-ttu-id="cb9b8-174">Du har två alternativ att välja mellan.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-174">You have two options to choose from.</span></span>

* <span data-ttu-id="cb9b8-175">**Alternativ 1. Migrera de virtuella datorerna till ett virtuellt nätverk skapas av plattform**</span><span class="sxs-lookup"><span data-stu-id="cb9b8-175">**Option 1. Migrate the VMs to a platform-created virtual network**</span></span>

    <span data-ttu-id="cb9b8-176">Verifiera först om du kan migrera Molntjänsten med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-176">First, validate if you can migrate the cloud service using the following commands:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    <span data-ttu-id="cb9b8-177">Föregående kommando visar alla varningar och fel som blockerar migrering.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-177">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="cb9b8-178">Om verifieringen lyckas, så du kan gå vidare till den **Förbered** steg:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-178">If validation is successful, then you can move on to the **Prepare** step:</span></span>

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* <span data-ttu-id="cb9b8-179">**Alternativ 2. Migrera till ett befintligt virtuellt nätverk i Resource Manager-distributionsmodellen**</span><span class="sxs-lookup"><span data-stu-id="cb9b8-179">**Option 2. Migrate to an existing virtual network in the Resource Manager deployment model**</span></span>

    <span data-ttu-id="cb9b8-180">Det här exemplet anger resursgruppens namn till **myResourceGroup**, det virtuella nätverksnamnet till **myVirtualNetwork** och undernätsnamn till **mySubNet**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-180">This example sets the resource group name to **myResourceGroup**, the virtual network name to **myVirtualNetwork** and the subnet name to **mySubNet**.</span></span> <span data-ttu-id="cb9b8-181">Ersätt namnen i exemplet med namnen på de egna resurser.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-181">Replace the names in the example with the names of your own resources.</span></span>

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    <span data-ttu-id="cb9b8-182">Verifiera först om du kan migrera det virtuella nätverket med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-182">First, validate if you can migrate the virtual network using the following command:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    <span data-ttu-id="cb9b8-183">Föregående kommando visar alla varningar och fel som blockerar migrering.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-183">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="cb9b8-184">Om verifieringen lyckas, kan du fortsätta med följande förberedelsesteget:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-184">If validation is successful, then you can proceed with the following Prepare step:</span></span>

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

<span data-ttu-id="cb9b8-185">När Förberedelseåtgärden lyckas med något av alternativen ovan, fråga migreringstillståndet för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-185">After the Prepare operation succeeds with either of the preceding options, query the migration state of the VMs.</span></span> <span data-ttu-id="cb9b8-186">Se till att de är i den `Prepared` tillstånd.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-186">Ensure that they are in the `Prepared` state.</span></span>

<span data-ttu-id="cb9b8-187">Det här exemplet anger namnet på virtuella datorn till **myVM**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-187">This example sets the VM name to **myVM**.</span></span> <span data-ttu-id="cb9b8-188">Ersätt exempelnamnet med ditt eget namn på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-188">Replace the example name with your own VM name.</span></span>

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

<span data-ttu-id="cb9b8-189">Kontrollera konfigurationen för de förberedda resurserna med hjälp av PowerShell eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-189">Check the configuration for the prepared resources by using either PowerShell or the Azure portal.</span></span> <span data-ttu-id="cb9b8-190">Om du inte är klar för migrering och du vill gå tillbaka till ett tidigare tillstånd, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-190">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

<span data-ttu-id="cb9b8-191">Om den förberedda konfigurationen ser bra ut du gå vidare och genomför resurserna med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-191">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="cb9b8-192">Steg 6.1: Alternativ 2 – migrera virtuella datorer i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="cb9b8-192">Step 6.1: Option 2 - Migrate virtual machines in a virtual network</span></span>

<span data-ttu-id="cb9b8-193">Om du vill migrera virtuella datorer i ett virtuellt nätverk, kan du migrera det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-193">To migrate virtual machines in a virtual network, you migrate the virtual network.</span></span> <span data-ttu-id="cb9b8-194">De virtuella datorerna migrera automatiskt med det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-194">The virtual machines automatically migrate with the virtual network.</span></span> <span data-ttu-id="cb9b8-195">Välj det virtuella nätverk som du vill migrera.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-195">Pick the virtual network that you want to migrate.</span></span>
> [!NOTE]
> <span data-ttu-id="cb9b8-196">[Migrera klassiska virtuell dator](migrate-single-classic-to-resource-manager.md) genom att skapa en ny Resource Manager virtuell dator med hanterade diskar med hjälp av VHD-(OS och data) filerna för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-196">[Migrate single classic virtual machine](migrate-single-classic-to-resource-manager.md) by creating a new Resource Manager virtual machine with Managed Disks using the VHD (OS and data) files of the virtual machine.</span></span>
<br>

> [!NOTE]
> <span data-ttu-id="cb9b8-197">Det virtuella nätverksnamnet kan skilja sig från vad som visas i den nya portalen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-197">The virtual network name might be different from what is shown in the new Portal.</span></span> <span data-ttu-id="cb9b8-198">Den nya Azure-portalen visar namn som `[vnet-name]` men det faktiska virtuella nätverksnamnet är av typen `Group [resource-group-name] [vnet-name]`.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-198">The new Azure Portal displays the name as `[vnet-name]` but the actual virtual network name is of type `Group [resource-group-name] [vnet-name]`.</span></span> <span data-ttu-id="cb9b8-199">Innan du migrerar, sökning faktiska virtuella nätverksnamn med hjälp av kommandot `Get-AzureVnetSite | Select -Property Name` eller visa i den gamla Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-199">Before migrating, lookup the actual virtual network name using the command `Get-AzureVnetSite | Select -Property Name` or view it in the old Azure Portal.</span></span> 

<span data-ttu-id="cb9b8-200">Det här exemplet anges det virtuella nätverksnamnet **myVnet**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-200">This example sets the virtual network name to **myVnet**.</span></span> <span data-ttu-id="cb9b8-201">Ersätt det virtuella nätverksnamnet exempel med din egen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-201">Replace the example virtual network name with your own.</span></span>

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> <span data-ttu-id="cb9b8-202">Om det virtuella nätverket innehåller webb- eller arbetsroller eller virtuella datorer med konfigurationer som inte stöds, kan du få ett felmeddelande för verifiering.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-202">If the virtual network contains web or worker roles, or VMs with unsupported configurations, you get a validation error message.</span></span>
>
>

<span data-ttu-id="cb9b8-203">Verifiera först om du kan migrera det virtuella nätverket med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-203">First, validate if you can migrate the virtual network by using the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

<span data-ttu-id="cb9b8-204">Föregående kommando visar alla varningar och fel som blockerar migrering.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-204">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="cb9b8-205">Om verifieringen lyckas, kan du fortsätta med följande förberedelsesteget:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-205">If validation is successful, then you can proceed with the following Prepare step:</span></span>

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

<span data-ttu-id="cb9b8-206">Kontrollera konfigurationen för förberedda virtuella datorer med hjälp av Azure PowerShell eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-206">Check the configuration for the prepared virtual machines by using either Azure PowerShell or the Azure portal.</span></span> <span data-ttu-id="cb9b8-207">Om du inte är klar för migrering och du vill gå tillbaka till ett tidigare tillstånd, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-207">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

<span data-ttu-id="cb9b8-208">Om den förberedda konfigurationen ser bra ut du gå vidare och genomför resurserna med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-208">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a><span data-ttu-id="cb9b8-209">Steg 6.2 Migrera ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="cb9b8-209">Step 6.2 Migrate a storage account</span></span>
<span data-ttu-id="cb9b8-210">När du är klar migrering av virtuella datorer, vi rekommenderar att du migrerar storage-konton.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-210">Once you're done migrating the virtual machines, we recommend you migrate the storage accounts.</span></span>

<span data-ttu-id="cb9b8-211">Utför föregående nödvändiga kontroller innan du migrerar storage-konto:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-211">Before you migrate the storage account, please perform preceding prerequisite checks:</span></span>

* <span data-ttu-id="cb9b8-212">**Migrera den klassiska virtuella datorer vars diskar lagras i storage-konto**</span><span class="sxs-lookup"><span data-stu-id="cb9b8-212">**Migrate classic virtual machines whose disks are stored in the storage account**</span></span>

    <span data-ttu-id="cb9b8-213">Föregående kommando returnerar RoleName och DiskName egenskaper för alla klassiska Virtuella diskar i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-213">Preceding command returns RoleName and DiskName properties of all the classic VM disks in the storage account.</span></span> <span data-ttu-id="cb9b8-214">RoleName är namnet på den virtuella datorn som en disk är ansluten.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-214">RoleName is the name of the virtual machine to which a disk is attached.</span></span> <span data-ttu-id="cb9b8-215">Om föregående kommando returnerar kontrollerar diskar att virtuella datorer till vilka diskarna är anslutna migreras innan du migrerar storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-215">If preceding command returns disks then ensure that virtual machines to which these disks are attached are migrated before migrating the storage account.</span></span>
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* <span data-ttu-id="cb9b8-216">**Ta bort beräkningsfunktionen klassiska Virtuella diskar som lagras i storage-konto**</span><span class="sxs-lookup"><span data-stu-id="cb9b8-216">**Delete unattached classic VM disks stored in the storage account**</span></span>

    <span data-ttu-id="cb9b8-217">Hitta beräkningsfunktionen klassiska Virtuella diskar i lagringen konto med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-217">Find unattached classic VM disks in the storage account using following command:</span></span>

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    <span data-ttu-id="cb9b8-218">Om ovanstående kommando returnerar diskar sedan ta bort dessa diskar med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-218">If above command returns disks then delete these disks using following command:</span></span>

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* <span data-ttu-id="cb9b8-219">**Ta bort VM-avbildningar lagras i storage-konto**</span><span class="sxs-lookup"><span data-stu-id="cb9b8-219">**Delete VM images stored in the storage account**</span></span>

    <span data-ttu-id="cb9b8-220">Föregående kommando returnerar alla VM-avbildningar med OS-disken som lagras i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-220">Preceding command returns all the VM images with OS disk stored in the storage account.</span></span>
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     <span data-ttu-id="cb9b8-221">Föregående kommando returnerar alla VM-avbildningar med datadiskar som lagras i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-221">Preceding command returns all the VM images with data disks stored in the storage account.</span></span>
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    <span data-ttu-id="cb9b8-222">Ta bort alla VM-avbildningar som returneras av ovan kommandon med hjälp av föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-222">Delete all the VM images returned by above commands using preceding command:</span></span>
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

<span data-ttu-id="cb9b8-223">Verifiera varje lagringskonto för migrering med hjälp av följande kommando.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-223">Validate each storage account for migration by using the following command.</span></span> <span data-ttu-id="cb9b8-224">I det här exemplet är lagringskontonamnet **Mittlagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-224">In this example, the storage account name is **myStorageAccount**.</span></span> <span data-ttu-id="cb9b8-225">Ersätt namnet på exemplet med namnet på ditt eget lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-225">Replace the example name with the name of your own storage account.</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

<span data-ttu-id="cb9b8-226">Nästa steg är att förbereda storage-konto för migrering</span><span class="sxs-lookup"><span data-stu-id="cb9b8-226">Next step is to Prepare the storage account for migration</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

<span data-ttu-id="cb9b8-227">Kontrollera konfigurationen för förberedda storage-konto med hjälp av Azure PowerShell eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cb9b8-227">Check the configuration for the prepared storage account by using either Azure PowerShell or the Azure portal.</span></span> <span data-ttu-id="cb9b8-228">Om du inte är klar för migrering och du vill gå tillbaka till ett tidigare tillstånd, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-228">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

<span data-ttu-id="cb9b8-229">Om den förberedda konfigurationen ser bra ut du gå vidare och genomför resurserna med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cb9b8-229">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a><span data-ttu-id="cb9b8-230">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb9b8-230">Next steps</span></span>
* [<span data-ttu-id="cb9b8-231">Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska till Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cb9b8-231">Overview of platform-supported migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="cb9b8-232">Tekniska ingående om plattformen stöds från klassiska till Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cb9b8-232">Technical deep dive on platform-supported migration from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="cb9b8-233">Planera för migrering av IaaS-resurser från klassisk till Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cb9b8-233">Planning for migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="cb9b8-234">Använda CLI för att migrera IaaS-resurser från klassiska till Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cb9b8-234">Use CLI to migrate IaaS resources from classic to Azure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="cb9b8-235">Community-verktyg för att hjälpa till med migrering av IaaS-resurser från klassiska till Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cb9b8-235">Community tools for assisting with migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="cb9b8-236">Granska de vanligaste migreringsfelen</span><span class="sxs-lookup"><span data-stu-id="cb9b8-236">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="cb9b8-237">Granska de vanligaste frågorna om migrera IaaS-resurser från klassiska till Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cb9b8-237">Review the most frequently asked questions about migrating IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
