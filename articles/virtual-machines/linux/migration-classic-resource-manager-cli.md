---
title: aaaMigrate VMs tooResource Manager med Azure CLI | Microsoft Docs
description: "Den här artikeln innehåller stegvisa hello stöds av plattformen migrering av resurser från klassiska tooAzure Resource Manager med hjälp av Azure CLI"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 0b11f4bb1b4658b3e88bf7629147ed953b678556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-cli"></a><span data-ttu-id="f2a02-103">Migrera IaaS-resurser från klassiska tooAzure Resource Manager med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f2a02-103">Migrate IaaS resources from classic tooAzure Resource Manager by using Azure CLI</span></span>
<span data-ttu-id="f2a02-104">Dessa steg visar hur toouse Azure-kommandoradsgränssnittet (CLI) kommandon toomigrate infrastruktur som en tjänst (IaaS)-resurser från hello klassisk distribution modellen toohello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f2a02-104">These steps show you how toouse Azure command-line interface (CLI) commands toomigrate infrastructure as a service (IaaS) resources from hello classic deployment model toohello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="f2a02-105">hello artikel kräver hello [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f2a02-105">hello article requires hello [Azure CLI](../../cli-install-nodejs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f2a02-106">Alla hello-åtgärder som beskrivs här är idempotent.</span><span class="sxs-lookup"><span data-stu-id="f2a02-106">All hello operations described here are idempotent.</span></span> <span data-ttu-id="f2a02-107">Om du har problem med än en funktion som inte stöds eller ett konfigurationsfel rekommenderar vi att du gör hello förbereda, avbrytas eller genomföras igen.</span><span class="sxs-lookup"><span data-stu-id="f2a02-107">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry hello prepare, abort, or commit operation.</span></span> <span data-ttu-id="f2a02-108">hello plattform försök sedan hello åtgärden igen.</span><span class="sxs-lookup"><span data-stu-id="f2a02-108">hello platform will then try hello action again.</span></span>
> 
> 

<br>
<span data-ttu-id="f2a02-109">Här är en flödesschema tooidentify hello ordning där steg måste toobe körs under en migreringsprocessen</span><span class="sxs-lookup"><span data-stu-id="f2a02-109">Here is a flowchart tooidentify hello order in which steps need toobe executed during a migration process</span></span>

![Skärmbild som visar hello migreringssteg](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a><span data-ttu-id="f2a02-111">Steg 1: Förbered för migrering</span><span class="sxs-lookup"><span data-stu-id="f2a02-111">Step 1: Prepare for migration</span></span>
<span data-ttu-id="f2a02-112">Här följer några metodtips som vi rekommenderar medan du utvärderar migrera IaaS-resurser från klassiska tooResource Manager:</span><span class="sxs-lookup"><span data-stu-id="f2a02-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic tooResource Manager:</span></span>

* <span data-ttu-id="f2a02-113">Läs igenom hello [lista över konfigurationer som inte stöds eller funktioner](../windows/migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2a02-113">Read through hello [list of unsupported configurations or features](../windows/migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="f2a02-114">Om du har virtuella datorer som använder icke-stödda konfigurationer eller funktioner, rekommenderar vi att du väntar hello/egenskapskonfiguration stöd toobe meddelats.</span><span class="sxs-lookup"><span data-stu-id="f2a02-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for hello feature/configuration support toobe announced.</span></span> <span data-ttu-id="f2a02-115">Du kan också ta bort funktionen eller flytta från att konfigurationen tooenable migreringen om den passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="f2a02-115">Alternatively, you can remove that feature or move out of that configuration tooenable migration if it suits your needs.</span></span>
* <span data-ttu-id="f2a02-116">Om du har automatiserat skript som distribuerar din infrastruktur och dina program idag, försök toocreate en liknande inställningar med hjälp av dessa skript för migrering.</span><span class="sxs-lookup"><span data-stu-id="f2a02-116">If you have automated scripts that deploy your infrastructure and applications today, try toocreate a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="f2a02-117">Du kan också ställa in exempel miljöer med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2a02-117">Alternatively, you can set up sample environments by using hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2a02-118">Programgatewayer stöds inte för migrering från klassiska tooResource Manager.</span><span class="sxs-lookup"><span data-stu-id="f2a02-118">Application Gateways are not currently supported for migration from classic tooResource Manager.</span></span> <span data-ttu-id="f2a02-119">toomigrate ett klassiskt virtuellt nätverk med en Programgateway ta bort hello gateway innan du kör ett förbereda åtgärden toomove hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="f2a02-119">toomigrate a classic virtual network with an Application gateway, remove hello gateway before running a Prepare operation toomove hello network.</span></span> <span data-ttu-id="f2a02-120">När du har slutfört migreringen hello återansluta hello gateway i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f2a02-120">After you complete hello migration, reconnect hello gateway in Azure Resource Manager.</span></span> 
>
><span data-ttu-id="f2a02-121">ExpressRoute-gatewayer som ansluter tooExpressRoute kretsar i en annan prenumeration som inte kan migreras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f2a02-121">ExpressRoute gateways connecting tooExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="f2a02-122">I sådana fall kan ta bort hello ExpressRoute-gateway, migrera hello virtuella nätverk och återskapa hello gateway.</span><span class="sxs-lookup"><span data-stu-id="f2a02-122">In such cases, remove hello ExpressRoute gateway, migrate hello virtual network and recreate hello gateway.</span></span> <span data-ttu-id="f2a02-123">Se [migrera ExpressRoute-kretsar och associerade virtuella nätverk från hello klassiska toohello Resource Manager-distributionsmodellen](../../expressroute/expressroute-migration-classic-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="f2a02-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from hello classic toohello Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
> 
> 

## <a name="step-2-set-your-subscription-and-register-hello-provider"></a><span data-ttu-id="f2a02-124">Steg 2: Ange din prenumeration och registrera hello-providern</span><span class="sxs-lookup"><span data-stu-id="f2a02-124">Step 2: Set your subscription and register hello provider</span></span>
<span data-ttu-id="f2a02-125">För Migreringsscenarier, behöver du tooset upp din miljö för både klassiska och Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f2a02-125">For migration scenarios, you need tooset up your environment for both classic and Resource Manager.</span></span> <span data-ttu-id="f2a02-126">[Installera Azure CLI](../../cli-install-nodejs.md) och [väljer din prenumeration](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="f2a02-126">[Install Azure CLI](../../cli-install-nodejs.md) and [select your subscription](../../xplat-cli-connect.md).</span></span>

<span data-ttu-id="f2a02-127">Logga in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="f2a02-127">Sign-in tooyour account.</span></span>

    azure login

<span data-ttu-id="f2a02-128">Välj hello Azure-prenumeration med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-128">Select hello Azure subscription by using hello following command.</span></span>

    azure account set "<azure-subscription-name>"

> [!NOTE]
> <span data-ttu-id="f2a02-129">Registreringen är en gång steg men behov toobe gjort en gång innan du försöker migrera.</span><span class="sxs-lookup"><span data-stu-id="f2a02-129">Registration is a one time step but it needs toobe done once before attempting migration.</span></span> <span data-ttu-id="f2a02-130">Utan att registrera visas följande felmeddelande hello</span><span class="sxs-lookup"><span data-stu-id="f2a02-130">Without registering you'll see hello following error message</span></span> 
> 
> <span data-ttu-id="f2a02-131">*BadRequest: Prenumerationen har inte registrerats för migrering.*</span><span class="sxs-lookup"><span data-stu-id="f2a02-131">*BadRequest : Subscription is not registered for migration.*</span></span> 
> 
> 

<span data-ttu-id="f2a02-132">Registrera med resursprovidern för hello migrering med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-132">Register with hello migration resource provider by using hello following command.</span></span> <span data-ttu-id="f2a02-133">Observera att i vissa fall kan det här kommandot på grund av timeout. Dock har hello registreringen lyckats.</span><span class="sxs-lookup"><span data-stu-id="f2a02-133">Note that in some cases, this command times out. However, hello registration will be successful.</span></span>

    azure provider register Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="f2a02-134">Vänta 5 minuter för hello registrering toofinish.</span><span class="sxs-lookup"><span data-stu-id="f2a02-134">Please wait five minutes for hello registration toofinish.</span></span> <span data-ttu-id="f2a02-135">Du kan kontrollera hello status för hello godkännandet med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-135">You can check hello status of hello approval by using hello following command.</span></span> <span data-ttu-id="f2a02-136">Se till att RegistrationState `Registered` innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="f2a02-136">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

    azure provider show Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="f2a02-137">Växla CLI toohello `asm` läge.</span><span class="sxs-lookup"><span data-stu-id="f2a02-137">Now switch CLI toohello `asm` mode.</span></span>

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="f2a02-138">Steg 3: Kontrollera att du har tillräckligt med Azure Resource Manager-dator kärnor i hello Azure-regionen för din aktuella distributionen eller virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="f2a02-138">Step 3: Make sure you have enough Azure Resource Manager Virtual Machine cores in hello Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="f2a02-139">För det här steget behöver du tooswitch för`arm` läge.</span><span class="sxs-lookup"><span data-stu-id="f2a02-139">For this step you'll need tooswitch too`arm` mode.</span></span> <span data-ttu-id="f2a02-140">Du kan göra detta med följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-140">Do this with hello following command.</span></span>

```
azure config mode arm
```

<span data-ttu-id="f2a02-141">Du kan använda hello följande CLI kommandot toocheck hello aktuell mängd kärnor som du har i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f2a02-141">You can use hello following CLI command toocheck hello current amount of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="f2a02-142">toolearn mer om grundläggande kvoter finns [gränser och hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span><span class="sxs-lookup"><span data-stu-id="f2a02-142">toolearn more about core quotas, see [Limits and hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span></span>

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

<span data-ttu-id="f2a02-143">När du har verifierat det här steget, kan du växla tillbaka för`asm` läge.</span><span class="sxs-lookup"><span data-stu-id="f2a02-143">Once you're done verifying this step, you can switch back too`asm` mode.</span></span>

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a><span data-ttu-id="f2a02-144">Steg 4: Alternativ 1 – migrera virtuella datorer i en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="f2a02-144">Step 4: Option 1 - Migrate virtual machines in a cloud service</span></span>
<span data-ttu-id="f2a02-145">Hämta hello lista över molntjänster med hjälp av följande kommando hello och välj hello molnbaserad tjänst som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="f2a02-145">Get hello list of cloud services by using hello following command, and then pick hello cloud service that you want toomigrate.</span></span> <span data-ttu-id="f2a02-146">Observera att om hello virtuella datorer i hello molntjänst är i ett virtuellt nätverk eller om de har web/worker-roller, du får ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="f2a02-146">Note that if hello VMs in hello cloud service are in a virtual network or if they have web/worker roles, you will get an error message.</span></span>

    azure service list

<span data-ttu-id="f2a02-147">Kör hello följande tooget hello distribution kommandonamnet för hello molnbaserad tjänst från hello utförlig utdata.</span><span class="sxs-lookup"><span data-stu-id="f2a02-147">Run hello following command tooget hello deployment name for hello cloud service from hello verbose output.</span></span> <span data-ttu-id="f2a02-148">I de flesta fall är hello distributionsnamnet hello samma som hello molntjänstnamnet.</span><span class="sxs-lookup"><span data-stu-id="f2a02-148">In most cases, hello deployment name is hello same as hello cloud service name.</span></span>

    azure service show <serviceName> -vv

<span data-ttu-id="f2a02-149">Verifiera först om du kan migrera hello Molntjänsten med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f2a02-149">First, validate if you can migrate hello cloud service using hello following commands:</span></span>

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

<span data-ttu-id="f2a02-150">Förbered hello virtuella datorer i hello Molntjänsten för migrering.</span><span class="sxs-lookup"><span data-stu-id="f2a02-150">Prepare hello virtual machines in hello cloud service for migration.</span></span> <span data-ttu-id="f2a02-151">Du har två alternativ toochoose från.</span><span class="sxs-lookup"><span data-stu-id="f2a02-151">You have two options toochoose from.</span></span>

<span data-ttu-id="f2a02-152">Om du vill toomigrate hello VMs tooa plattform skapade virtuella nätverk använder du följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-152">If you want toomigrate hello VMs tooa platform-created virtual network, use hello following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

<span data-ttu-id="f2a02-153">Om du vill toomigrate tooan befintliga virtuella nätverk i hello Resource Manager-distributionsmodellen använder du följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-153">If you want toomigrate tooan existing virtual network in hello Resource Manager deployment model, use hello following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

<span data-ttu-id="f2a02-154">När hello förberett lyckas, kan du titta igenom hello utförlig utdata tooget hello migrering tillstånd hello virtuella datorer och så att de är i hello `Prepared` tillstånd.</span><span class="sxs-lookup"><span data-stu-id="f2a02-154">After hello prepare operation is successful, you can look through hello verbose output tooget hello migration state of hello VMs and ensure that they are in hello `Prepared` state.</span></span>

    azure vm show <vmName> -vv

<span data-ttu-id="f2a02-155">Kontrollera hello-konfigurationen för hello förberedd resurser med hjälp av CLI eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2a02-155">Check hello configuration for hello prepared resources by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="f2a02-156">Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="f2a02-156">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure service deployment abort-migration <serviceName> <deploymentName>

<span data-ttu-id="f2a02-157">Om hello förberedda konfigurationen ser bra ut kan du gå vidare och genomför hello resurser med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-157">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="f2a02-158">Steg 4: Alternativ 2 – migrera virtuella datorer i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="f2a02-158">Step 4: Option 2 -  Migrate virtual machines in a virtual network</span></span>
<span data-ttu-id="f2a02-159">Välj hello virtuella nätverk som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="f2a02-159">Pick hello virtual network that you want toomigrate.</span></span> <span data-ttu-id="f2a02-160">Observera att om hello virtuellt nätverk innehåller web/worker-roller eller virtuella datorer med konfigurationer som inte stöds, visas ett felmeddelande för verifiering.</span><span class="sxs-lookup"><span data-stu-id="f2a02-160">Note that if hello virtual network contains web/worker roles or VMs with unsupported configurations, you will get a validation error message.</span></span>

<span data-ttu-id="f2a02-161">Hämta alla hello virtuella nätverk i hello prenumeration med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-161">Get all hello virtual networks in hello subscription by using hello following command.</span></span>

    azure network vnet list

<span data-ttu-id="f2a02-162">hello utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="f2a02-162">hello output will look something like this:</span></span>

![Skärmbild av hello kommandoraden med hello hela virtuella nätverksnamnet markerat.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

<span data-ttu-id="f2a02-164">I ovanstående exempel hello, hello **virtualNetworkName** är hello hela namnet **”grupp classicubuntu16 classicubuntu16”**.</span><span class="sxs-lookup"><span data-stu-id="f2a02-164">In hello above example, hello **virtualNetworkName** is hello entire name **"Group classicubuntu16 classicubuntu16"**.</span></span>

<span data-ttu-id="f2a02-165">Verifiera först om du kan migrera hello virtuellt nätverk med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f2a02-165">First, validate if you can migrate hello virtual network using hello following command:</span></span>

```shell
azure network vnet validate-migration <virtualNetworkName>
```

<span data-ttu-id="f2a02-166">Förbered hello virtuellt nätverk önskat för migrering med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-166">Prepare hello virtual network of your choice for migration by using hello following command.</span></span>

    azure network vnet prepare-migration <virtualNetworkName>

<span data-ttu-id="f2a02-167">Kontrollera hello-konfigurationen för hello förbereda virtuella datorer med hjälp av CLI eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2a02-167">Check hello configuration for hello prepared virtual machines by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="f2a02-168">Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="f2a02-168">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure network vnet abort-migration <virtualNetworkName>

<span data-ttu-id="f2a02-169">Om hello förberedda konfigurationen ser bra ut kan du gå vidare och genomför hello resurser med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-169">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a><span data-ttu-id="f2a02-170">Steg 5: Migrera ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="f2a02-170">Step 5: Migrate a storage account</span></span>
<span data-ttu-id="f2a02-171">När du är klar migrera hello virtuella datorer, rekommenderar vi att du migrerar hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f2a02-171">Once you're done migrating hello virtual machines, we recommend you migrate hello storage account.</span></span>

<span data-ttu-id="f2a02-172">Förbereda hello storage-konto för migrering med hjälp av följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="f2a02-172">Prepare hello storage account for migration by using hello following command</span></span>

    azure storage account prepare-migration <storageAccountName>

<span data-ttu-id="f2a02-173">Kontrollera hello-konfigurationen för hello förberedd storage-konto med hjälp av CLI eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2a02-173">Check hello configuration for hello prepared storage account by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="f2a02-174">Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="f2a02-174">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure storage account abort-migration <storageAccountName>

<span data-ttu-id="f2a02-175">Om hello förberedda konfigurationen ser bra ut kan du gå vidare och genomför hello resurser med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="f2a02-175">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a><span data-ttu-id="f2a02-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f2a02-176">Next steps</span></span>

* [<span data-ttu-id="f2a02-177">Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2a02-177">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="f2a02-178">Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2a02-178">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="f2a02-179">Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2a02-179">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="f2a02-180">Använd PowerShell toomigrate IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2a02-180">Use PowerShell toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f2a02-181">Community-verktyg för att hjälpa till med migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2a02-181">Community tools for assisting with migration of IaaS resources from classic tooAzure Resource Manager</span></span>](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f2a02-182">Granska de vanligaste migreringsfelen</span><span class="sxs-lookup"><span data-stu-id="f2a02-182">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="f2a02-183">Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f2a02-183">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
