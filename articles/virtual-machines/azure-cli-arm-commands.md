---
title: "aaaAzure CLI-kommandon i Resource Manager-läget | Microsoft Docs"
description: "Azure kommandoradsgränssnittet (CLI) kommandon toomanage resurser i hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux,virtual-machines-windows,virtual-network,mobile-services,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: be37da5b-72fe-41a1-9fa0-8937b69464ec
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: danlep
ms.openlocfilehash: 49539655f7b24511e219f982819bcb59c9305d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-commands-in-resource-manager-mode"></a><span data-ttu-id="d0f9e-103">Azure CLI-kommandona i Resource Manager-läge</span><span class="sxs-lookup"><span data-stu-id="d0f9e-103">Azure CLI commands in Resource Manager mode</span></span>
<span data-ttu-id="d0f9e-104">Den här artikeln innehåller syntax och alternativ för Azure-kommandoradsgränssnittet (CLI)-kommandon du kan ofta använda toocreate och hantera Azure-resurser i hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-104">This article provides syntax and options for Azure command-line interface (CLI) commands you'd commonly use toocreate and manage Azure resources in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="d0f9e-105">Du har åtkomst till dessa kommandon genom att köra hello CLI i Resource Manager (arm)-läge.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-105">You access these commands by running hello CLI in Resource Manager (arm) mode.</span></span> <span data-ttu-id="d0f9e-106">Detta är inte en fullständig referens och CLI-versionen kan indikera att något annorlunda kommandon eller parametrar.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-106">This is not a complete reference, and your CLI version may show slightly different commands or parameters.</span></span> <span data-ttu-id="d0f9e-107">En allmän översikt över Azure-resurser och resursgrupper finns i [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d0f9e-107">For a general overview of Azure resources and resource groups, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="d0f9e-108">Detta kallas ibland artikeln visar Resource Manager kommandon läge i hello Azure CLI, Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-108">This article shows Resource Manager mode commands in hello Azure CLI, sometimes called Azure CLI 1.0.</span></span> <span data-ttu-id="d0f9e-109">toowork i hello Resource Manager-modellen, du kan också försöka hello [Azure CLI 2.0](/cli/azure/install-az-cli2), våra nästa generation multiplattform CLI.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-109">toowork in hello Resource Manager model, you can also try hello [Azure CLI 2.0](/cli/azure/install-az-cli2), our next generation multi-platform CLI.</span></span>
><span data-ttu-id="d0f9e-110">Lär dig mer om hello [gamla och nya Azure CLIs](/cli/azure/old-and-new-clis).</span><span class="sxs-lookup"><span data-stu-id="d0f9e-110">Find out more about hello [old and new Azure CLIs](/cli/azure/old-and-new-clis).</span></span>
>

<span data-ttu-id="d0f9e-111">tooget igång först [installera hello Azure CLI](../cli-install-nodejs.md) och [ansluta tooyour Azure-prenumeration](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="d0f9e-111">tooget started, first [install hello Azure CLI](../cli-install-nodejs.md) and [connect tooyour Azure subscription](../xplat-cli-connect.md).</span></span>

<span data-ttu-id="d0f9e-112">Skriv den aktuella kommandosyntax och alternativ på hello kommandoraden i Resource Manager-läget `azure help` eller toodisplay hjälp för ett specifikt kommando `azure help [command]`.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-112">For current command syntax and options at hello command line in Resource Manager mode, type `azure help` or, toodisplay help for a specific command, `azure help [command]`.</span></span> <span data-ttu-id="d0f9e-113">Även kan hitta CLI-exempel i hello-dokumentationen för att skapa och hantera specifika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-113">Also find CLI examples in hello documentation for creating and managing specific Azure services.</span></span>

<span data-ttu-id="d0f9e-114">Valfria parametrar visas inom hakparentes (till exempel `[parameter]`).</span><span class="sxs-lookup"><span data-stu-id="d0f9e-114">Optional parameters are shown in square brackets (for example, `[parameter]`).</span></span> <span data-ttu-id="d0f9e-115">Alla andra parametrar är obligatoriska.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-115">All other parameters are required.</span></span>

<span data-ttu-id="d0f9e-116">Tillägg toocommand-specifika valfria parametrar dokumenteras här, finns det tre valfria parametrar som kan använda toodisplay detaljerade utdata, till exempel alternativen och statuskoder.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-116">In addition toocommand-specific optional parameters documented here, there are three optional parameters that can be used toodisplay detailed output such as request options and status codes.</span></span> <span data-ttu-id="d0f9e-117">Hej `-v` parametern innehåller utförliga data och hello `-vv` parametern innehåller även detaljerad utförlig utdata.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-117">hello `-v` parameter provides verbose output, and hello `-vv` parameter provides even more detailed verbose output.</span></span> <span data-ttu-id="d0f9e-118">Hej `--json` alternativet matar ut hello resultatet i raw json-format.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-118">hello `--json` option outputs hello result in raw json format.</span></span>

## <a name="setting-hello-resource-manager-mode"></a><span data-ttu-id="d0f9e-119">Inställningen hello Resource Manager-läge</span><span class="sxs-lookup"><span data-stu-id="d0f9e-119">Setting hello Resource Manager mode</span></span>
<span data-ttu-id="d0f9e-120">Använd följande kommandon för Azure CLI Resource Manager-läge i kommandot tooenable hello.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-120">Use hello following command tooenable Azure CLI Resource Manager mode commands.</span></span>

    azure config mode arm

> [!NOTE]
> <span data-ttu-id="d0f9e-121">hello CLI Azure Resource Manager och Azure Service Management-läge kan inte anges samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-121">hello CLI's Azure Resource Manager mode and Azure Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="d0f9e-122">Det vill säga resurser som skapats i ett läge kan inte hanteras från hello andra läge.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-122">That is, resources created in one mode cannot be managed from hello other mode.</span></span>
> 
> 

## <a name="azure-account-manage-your-account-information"></a><span data-ttu-id="d0f9e-123">Azure-konto: hantera din kontoinformation</span><span class="sxs-lookup"><span data-stu-id="d0f9e-123">azure account: Manage your account information</span></span>
<span data-ttu-id="d0f9e-124">Din Azure-prenumerationsinformation används av hello verktyget tooconnect tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-124">Your Azure subscription information is used by hello tool tooconnect tooyour account.</span></span>

<span data-ttu-id="d0f9e-125">**Lista hello importeras prenumerationer**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-125">**List hello imported subscriptions**</span></span>

    account list [options]

<span data-ttu-id="d0f9e-126">**Visa information om en prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-126">**Show details about a subscription**</span></span>  

    account show [options] [subscriptionNameOrId]

<span data-ttu-id="d0f9e-127">**Ange hello aktuell prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-127">**Set hello current subscription**</span></span>

    account set [options] <subscriptionNameOrId>

<span data-ttu-id="d0f9e-128">**Ta bort en prenumeration eller miljö, eller avmarkera alla hello lagras information om kontot och miljö**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-128">**Remove a subscription or environment, or clear all of hello stored account and environment info**</span></span>  

    account clear [options]

<span data-ttu-id="d0f9e-129">**Kommandon toomanage konto miljön**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-129">**Commands toomanage your account environment**</span></span>  

    account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]

## <a name="azure-ad-commands-toodisplay-active-directory-objects"></a><span data-ttu-id="d0f9e-130">Azure ad: kommandon toodisplay Active Directory-objekt</span><span class="sxs-lookup"><span data-stu-id="d0f9e-130">azure ad: Commands toodisplay Active Directory objects</span></span>
<span data-ttu-id="d0f9e-131">**Kommandon toodisplay active directory-program**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-131">**Commands toodisplay active directory applications**</span></span>

    ad app create [options]
    ad app delete [options] <object-id>

<span data-ttu-id="d0f9e-132">**Kommandon toodisplay active directory-grupper**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-132">**Commands toodisplay active directory groups**</span></span>

    ad group list [options]
    ad group show [options]

<span data-ttu-id="d0f9e-133">**Kommandon tooprovide en active directory sub gruppen eller medlemmen info**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-133">**Commands tooprovide an active directory sub group or member info**</span></span>

    ad group member list [options] [objectId]

<span data-ttu-id="d0f9e-134">**Kommandon toodisplay active directory tjänstens huvudnamn**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-134">**Commands toodisplay active directory service principals**</span></span>

    ad sp list [options]
    ad sp show [options]
    ad sp create [options] <application-id>
    ad sp delete [options] <object-id>

<span data-ttu-id="d0f9e-135">**Kommandon toodisplay active directory-användare**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-135">**Commands toodisplay active directory users**</span></span>

    ad user list [options]
    ad user show [options]

## <a name="azure-availset-commands-toomanage-your-availability-sets"></a><span data-ttu-id="d0f9e-136">Azure availset: kommandon toomanage din tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="d0f9e-136">azure availset: commands toomanage your availability sets</span></span>
<span data-ttu-id="d0f9e-137">**Skapar en tillgänglighetsuppsättning inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-137">**Creates an availability set within a resource group**</span></span>

    availset create [options] <resource-group> <name> <location> [tags]

<span data-ttu-id="d0f9e-138">**Visar hello tillgänglighetsuppsättningar inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-138">**Lists hello availability sets within a resource group**</span></span>

    availset list [options] <resource-group>

<span data-ttu-id="d0f9e-139">**Hämtar en tillgänglighetsuppsättning inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-139">**Gets one availability set within a resource group**</span></span>

    availset show [options] <resource-group> <name>

<span data-ttu-id="d0f9e-140">**Tar bort en tillgänglighetsuppsättning inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-140">**Deletes one availability set within a resource group**</span></span>

    availset delete [options] <resource-group> <name>

## <a name="azure-config-commands-toomanage-your-local-settings"></a><span data-ttu-id="d0f9e-141">Azure config: kommandon toomanage dina lokala inställningar</span><span class="sxs-lookup"><span data-stu-id="d0f9e-141">azure config: commands toomanage your local settings</span></span>
<span data-ttu-id="d0f9e-142">**Konfigurationsinställningar för listan Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-142">**List Azure CLI configuration settings**</span></span>

    config list [options]

<span data-ttu-id="d0f9e-143">**Ta bort en konfigurationsinställning**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-143">**Delete a config setting**</span></span>

    config delete [options] <name>

<span data-ttu-id="d0f9e-144">**Uppdatera en konfigurationsinställning**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-144">**Update a config setting**</span></span>

    config set <name> <value>

<span data-ttu-id="d0f9e-145">**Anger hello Azure CLI fungerar läge tooeither `arm` eller`asm`**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-145">**Sets hello Azure CLI working mode tooeither `arm` or `asm`**</span></span>

    config mode [options] <modename>


## <a name="azure-feature-commands-toomanage-account-features"></a><span data-ttu-id="d0f9e-146">Azure-funktionen: kommandon toomanage kontofunktioner</span><span class="sxs-lookup"><span data-stu-id="d0f9e-146">azure feature: commands toomanage account features</span></span>
<span data-ttu-id="d0f9e-147">**Visa en lista över alla funktioner som är tillgänglig för din prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-147">**List all features available for your subscription**</span></span>

    feature list [options]

<span data-ttu-id="d0f9e-148">**Visar en funktion**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-148">**Shows a feature**</span></span>

    feature show [options] <providerName> <featureName>

<span data-ttu-id="d0f9e-149">**Registrerar en förhandsgranskade funktion i en resursleverantör**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-149">**Registers a previewed feature of a resource provider**</span></span>

    feature register [options] <providerName> <featureName>

## <a name="azure-group-commands-toomanage-your-resource-groups"></a><span data-ttu-id="d0f9e-150">Azure-grupp: kommandon toomanage resursgrupperna</span><span class="sxs-lookup"><span data-stu-id="d0f9e-150">azure group: Commands toomanage your resource groups</span></span>
<span data-ttu-id="d0f9e-151">**Skapar en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-151">**Creates a resource group**</span></span>

    group create [options] <name> <location>

<span data-ttu-id="d0f9e-152">**Ange taggar tooa resursgruppen.**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-152">**Set tags tooa resource group**</span></span>

    group set [options] <name> <tags>

<span data-ttu-id="d0f9e-153">**Tar bort en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-153">**Deletes a resource group**</span></span>

    group delete [options] <name>

<span data-ttu-id="d0f9e-154">**Visar hello resursgrupper för din prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-154">**Lists hello resource groups for your subscription**</span></span>

    group list [options]

<span data-ttu-id="d0f9e-155">**Visar en resursgrupp för din prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-155">**Shows a resource group for your subscription**</span></span>

    group show [options] <name>

<span data-ttu-id="d0f9e-156">**Kommandon toomanage resurs loggar**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-156">**Commands toomanage resource group logs**</span></span>

    group log show [options] [name]

<span data-ttu-id="d0f9e-157">**Kommandon toomanage distributionen i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-157">**Commands toomanage your deployment in a resource group**</span></span>

    group deployment create [options] [resource-group] [name]
    group deployment list [options] <resource-group> [state]
    group deployment show [options] <resource-group> [deployment-name]
    group deployment stop [options] <resource-group> [deployment-name]

<span data-ttu-id="d0f9e-158">**Kommandon toomanage mallen lokal eller gallery resource grupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-158">**Commands toomanage your local or gallery resource group template**</span></span>

    group template list [options]
    group template show [options] <name>
    group template download [options] [name] [file]
    group template validate [options] <resource-group>

## <a name="azure-hdinsight-commands-toomanage-your-hdinsight-clusters"></a><span data-ttu-id="d0f9e-159">Azure hdinsight: kommandon toomanage HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="d0f9e-159">azure hdinsight: Commands toomanage your HDInsight clusters</span></span>
<span data-ttu-id="d0f9e-160">**Kommandon toocreate eller lägga till tooa kluster-konfigurationsfil**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-160">**Commands toocreate or add tooa cluster configuration file**</span></span>

    hdinsight config create [options] <configFilePath> <overwrite>
    hdinsight config add-config-values [options] <configFilePath>
    hdinsight config add-script-action [options] <configFilePath>

<span data-ttu-id="d0f9e-161">Exempel: Skapa en konfigurationsfil som innehåller ett skript åtgärd toorun när du skapar ett kluster.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-161">Example: Create a configuration file that contains a script action toorun when creating a cluster.</span></span>

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

<span data-ttu-id="d0f9e-162">**Kommandot toocreate ett kluster i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-162">**Command toocreate a cluster in a resource group**</span></span>

    hdinsight cluster create [options] <clusterName>

<span data-ttu-id="d0f9e-163">Exempel: Skapa ett Storm på Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="d0f9e-163">Example: Create a Storm on Linux cluster</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting hello request toocreate cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="d0f9e-164">Exempel: Skapa ett kluster med en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="d0f9e-164">Example: Create a cluster with a script action</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting hello request toocreate cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="d0f9e-165">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-165">Parameter options:</span></span>

    -h, --help                                                 output usage information
    -v, --verbose                                              use verbose output
    -vv                                                        more verbose with debug output
    --json                                                     use json output
    -g --resource-group <resource-group>                       hello name of hello resource group
    -c, --clusterName <clusterName>                            HDInsight cluster name
    -l, --location <location>                                  Data center location for hello cluster
    -y, --osType <osType>                                      HDInsight cluster operating system
    'Windows' or 'Linux'
    --version <version>                                        HDInsight cluster version
    --clusterType <clusterType>                                HDInsight cluster type.
    Hadoop | HBase | Spark | Storm
    --defaultStorageAccountName <storageAccountName>           Storage account url toouse for default HDInsight storage
    --defaultStorageAccountKey <storageAccountKey>             Key toohello storage account toouse for default HDInsight storage
    --defaultStorageContainer <storageContainer>               Container in hello storage account toouse for HDInsight default storage
    --headNodeSize <headNodeSize>                              (Optional) Head node size for hello cluster
    --workerNodeCount <workerNodeCount>                        Number of worker nodes toouse for hello cluster
    --workerNodeSize <workerNodeSize>                          (Optional) Worker node size for hello cluster)
    --zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for hello cluster
    --userName <userName>                                      Cluster username
    --password <password>                                      Cluster password
    --sshUserName <sshUserName>                                SSH username (only for Linux clusters)
    --sshPassword <sshPassword>                                SSH password (only for Linux clusters)
    --sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
    --rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
    --rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
    --rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
    For example 12/12/2015 (only for Windows clusters)
    --virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for hello cluster.
    Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
    --subnetName <subnetName>                                  (Optional) Subnet for hello cluster
    --additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
    Can be multiple.
    In hello format of 'accountName#accountKey'.
    For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
    --hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for hello external metastore for Hive
    --hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for hello external metastore for Hive
    --hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for hello external metastore for Hive
    --hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for hello external metastore for Hive
    --oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for hello external metastore for Oozie
    --oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for hello external metastore for Oozie
    --oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for hello external metastore for Oozie
    --oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for hello external metastore for Oozie
    --configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
    -s, --subscription <id>                                    hello subscription id
    --tags <tags>                                              Tags tooset toohello cluster.
    Can be multiple.
    In hello format of 'name=value'.
    Name is required and value is optional.
    For example, --tags tag1=value1;tag2


<span data-ttu-id="d0f9e-166">**Kommandot toodelete ett kluster**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-166">**Command toodelete a cluster**</span></span>

    hdinsight cluster delete [options] <clusterName>

<span data-ttu-id="d0f9e-167">**Information om kommandot tooshow kluster**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-167">**Command tooshow cluster details**</span></span>

    hdinsight cluster show [options] <clusterName>

<span data-ttu-id="d0f9e-168">**Kommandot toolist alla kluster (i en viss resursgrupp, om angett)**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-168">**Command toolist all clusters (in a specific resource group, if provided)**</span></span>

    hdinsight cluster list [options]

<span data-ttu-id="d0f9e-169">**Kommandot tooresize ett kluster**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-169">**Command tooresize a cluster**</span></span>

    hdinsight cluster resize [options] <clusterName> <targetInstanceCount>

<span data-ttu-id="d0f9e-170">**Kommandot tooenable HTTP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-170">**Command tooenable HTTP access for a cluster**</span></span>

    hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>

<span data-ttu-id="d0f9e-171">**Kommandot toodisable HTTP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-171">**Command toodisable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-http-access [options] <clusterName>

<span data-ttu-id="d0f9e-172">**Kommandot tooenable RDP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-172">**Command tooenable RDP access for a cluster**</span></span>

    hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>

<span data-ttu-id="d0f9e-173">**Kommandot toodisable HTTP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-173">**Command toodisable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-rdp-access [options] <clusterName>

## <a name="azure-insights-commands-related-toomonitoring-insights-events-alert-rules-autoscale-settings-metrics"></a><span data-ttu-id="d0f9e-174">Azure insikter: kommandon relaterade toomonitoring insikter (händelser, Varningsregler, Autoskala inställningar, mått)</span><span class="sxs-lookup"><span data-stu-id="d0f9e-174">azure insights: Commands related toomonitoring Insights (events, alert rules, autoscale settings, metrics)</span></span>
<span data-ttu-id="d0f9e-175">**Hämta åtgärdsloggar för en prenumeration, en correlationId, en resursgrupp, resurs eller resursprovidern**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-175">**Retrieve operation logs for a subscription, a correlationId, a resource group, resource, or resource provider**</span></span>

    insights logs list [options]

## <a name="azure-location-commands-tooget-hello-available-locations-for-all-resource-types"></a><span data-ttu-id="d0f9e-176">Azure-plats: kommandon tooget hello tillgängliga platser för alla typer av resurser</span><span class="sxs-lookup"><span data-stu-id="d0f9e-176">azure location: Commands tooget hello available locations for all resource types</span></span>
<span data-ttu-id="d0f9e-177">**Lista hello tillgängliga platser**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-177">**List hello available locations**</span></span>

    location list [options]

## <a name="azure-network-commands-toomanage-network-resources"></a><span data-ttu-id="d0f9e-178">Azure-nätverk: kommandon toomanage nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="d0f9e-178">azure network: Commands toomanage network resources</span></span>
<span data-ttu-id="d0f9e-179">**Kommandon toomanage virtuella nätverk**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-179">**Commands toomanage virtual networks**</span></span>

    network vnet create [options] <resource-group> <name> <location>
<span data-ttu-id="d0f9e-180">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-180">Creates a virtual network.</span></span> <span data-ttu-id="d0f9e-181">I hello namnet newvnet för resursen grupp myresourcegroup i hello västra USA region i följande exempel skapar vi ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-181">In hello following example we create a virtual network named newvnet for resource group myresourcegroup in hello West US region.</span></span>

    azure network vnet create myresourcegroup newvnet "west us"
    info:    Executing command network vnet create
    + Looking up virtual network "newvnet"
    + Creating virtual network "newvnet"
     Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet create command OK


<span data-ttu-id="d0f9e-182">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-182">Parameter options:</span></span>

     -h, --help                                 output usage information
     -v, --verbose                              use verbose output
    --json                                     use json output
     -g, --resource-group <resource-group>      hello name of hello resource group
     -n, --name <name>                          hello name of hello virtual network
     -l, --location <location>                  hello location
     -a, --address-prefixes <address-prefixes>  hello comma separated list of address prefixes for this virtual network
      For example -a 10.0.0.0/24,10.0.1.0/24.
      Default value is 10.0.0.0/8

    -d, --dns-servers <dns-servers>            hello comma separated list of DNS servers IP addresses
     -t, --tags <tags>                          hello tags set on this virtual network.
      Can be multiple. In hello format of "name=value".
      Name is required and value is optional.
      For example, -t tag1=value1;tag2
     -s, --subscription <subscription>          hello subscription identifier
<BR>

    network vnet set [options] <resource-group> <name>

<span data-ttu-id="d0f9e-183">Uppdaterar en konfiguration av virtuellt nätverk inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-183">Updates a virtual network configuration within a resource group.</span></span>

    azure network vnet set myresourcegroup newvnet

    info:    Executing command network vnet set
    + Looking up virtual network "newvnet"
    + Updating virtual network "newvnet"
    + Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet set command OK

<span data-ttu-id="d0f9e-184">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-184">Parameter options:</span></span>

       -h, --help                                 output usage information
       -v, --verbose                              use verbose output
       --json                                     use json output
       -g, --resource-group <resource-group>      hello name of hello resource group
       -n, --name <name>                          hello name of hello virtual network
       -a, --address-prefixes <address-prefixes>  hello comma separated list of address prefixes for this virtual network.
        For example -a 10.0.0.0/24,10.0.1.0/24.
        This list will be appended toohello current list of address prefixes.
        hello address prefixes in this list should not overlap between them.
        hello address prefixes in this list should not overlap with existing address prefixes in hello vnet.

       -d, --dns-servers [dns-servers]            hello comma separated list of DNS servers IP addresses.
        This list will be appended toohello current list of DNS server IP addresses.

       -t, --tags <tags>                          hello tags set on this virtual network.
        Can be multiple. In hello format of "name=value".
        Name is required and value is optional. For example, -t tag1=value1;tag2.
        This list will be appended toohello current list of tags

       --no-tags                                  remove all existing tags
       -s, --subscription <subscription>          hello subscription identifier
<BR>

    network vnet list [options] <resource-group>

<span data-ttu-id="d0f9e-185">hello kommando visar alla virtuella nätverk i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-185">hello command lists all virtual networks in a resource group.</span></span>

    C:\>azure network vnet list myresourcegroup

    info:    Executing command network vnet list
    + Listing virtual networks
        data:    ID
       Name      Location  Address prefixes  DNS servers
    data:    -------------------------------------------------------------------
    ------  --------  --------  ----------------  -----------
    data:    /subscriptions/###############################/resourceGroups/
    wvnet   newvnet   westus    10.0.0.0/8
    info:    network vnet list command OK

<span data-ttu-id="d0f9e-186">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-186">Parameter options:</span></span>

      -h, --help                             output usage information
      -v, --verbose                          use verbose output
      --json                                 use json output
      -g, --resource-group <resource-group>  hello name of hello resource group
      -s, --subscription <subscription>      hello subscription identifier

<BR>

    network vnet show [options] <resource-group> <name>
<span data-ttu-id="d0f9e-187">hello kommando visar hello egenskaper för virtuellt nätverk i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-187">hello command shows hello virtual network properties in a resource group.</span></span>

    azure network vnet show -g myresourcegroup -n newvnet

    info:    Executing command network vnet show
    + Looking up virtual network "newvnet"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet show command OK
<BR>

    network vnet delete [options] <resource-group> <name>
<span data-ttu-id="d0f9e-188">hello kommandot tar bort ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-188">hello command removes a virtual network.</span></span>

    azure network vnet delete myresourcegroup newvnetX

    info:    Executing command network vnet delete
    + Looking up virtual network "newvnetX"
    Delete virtual network newvnetX? [y/n] y
    + Deleting virtual network "newvnetX"
    info:    network vnet delete command OK

<span data-ttu-id="d0f9e-189">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-189">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -n, --name <name>                      hello name of hello virtual network
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      hello subscription identifier


<span data-ttu-id="d0f9e-190">**Kommandon toomanage virtuellt undernät**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-190">**Commands toomanage virtual network subnets**</span></span>

    network vnet subnet create [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="d0f9e-191">Lägger till ett annat undernät tooan befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-191">Adds another subnet tooan existing virtual network.</span></span>

    azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

    info:    Executing command network vnet subnet create
    + Looking up hello subnet "subnet"
    + Creating subnet "subnet"
    + Looking up hello subnet "subnet"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Name:                      subnet
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet create command OK

<span data-ttu-id="d0f9e-192">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-192">Parameter options:</span></span>

     -h, --help                                                       output usage information
     -v, --verbose                                                    use verbose output
         --json                                                           use json output
     -g, --resource-group <resource-group>                            hello name of hello resource group
     -e, --vnet-name <vnet-name>                                      hello name of hello virtual network
     -n, --name <name>                                                hello name of hello subnet
     -a, --address-prefix <address-prefix>                            hello address prefix
     -w, --network-security-group-id <network-security-group-id>      hello network security group identifier.
           e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
     -o, --network-security-group-name <network-security-group-name>  hello network security group name
     -s, --subscription <subscription>                                hello subscription identifier

<BR>

    network vnet subnet set [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="d0f9e-193">Anger ett specifikt virtuellt undernät inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-193">Sets a specific virtual network subnet within a resource group.</span></span>

    C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet list [options] <resource-group> <vnet-name>

<span data-ttu-id="d0f9e-194">Listar alla hello virtuellt undernät för ett virtuellt nätverk inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-194">Lists all hello virtual network subnets for a specific virtual network within a resource group.</span></span>

    azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet show [options] <resource-group> <vnet-name> <name>
<span data-ttu-id="d0f9e-195">Visar egenskaper för undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="d0f9e-195">Displays virtual network subnet properties</span></span>

    azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet show
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
    .Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet show command OK

<span data-ttu-id="d0f9e-196">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-196">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -e, --vnet-name <vnet-name>            hello name of hello virtual network
    -n, --name <name>                      hello name of hello subnet
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
<span data-ttu-id="d0f9e-197">Tar bort ett undernät från ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-197">Removes a subnet from an existing virtual network.</span></span>

    azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet delete
    + Looking up hello subnet "subnet1"
    Delete subnet "subnet1"? [y/n] y
    + Deleting subnet "subnet1"
    info:    network vnet subnet delete command OK

<span data-ttu-id="d0f9e-198">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-198">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -e, --vnet-name <vnet-name>            hello name of hello virtual network
     -n, --name <name>                      hello subnet name
     -s, --subscription <subscription>      hello subscription identifier
     -q, --quiet                            quiet mode, do not ask for delete confirmation

<span data-ttu-id="d0f9e-199">**Kommandon toomanage belastningsutjämnare**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-199">**Commands toomanage load balancers**</span></span>

    network lb create [options] <resource-group> <name> <location>
<span data-ttu-id="d0f9e-200">Skapar en belastningen belastningsutjämnaren mängd.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-200">Creates a load balancer set.</span></span>

    azure network lb create -g myresourcegroup -n mylb -l westus

    info:    Executing command network lb create
    + Looking up hello load balancer "mylb"
    + Creating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb create command OK

<span data-ttu-id="d0f9e-201">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-201">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello load balancer
    -l, --location <location>              hello location
    -t, --tags <tags>                      hello list of tags.
     Can be multiple. In hello format of "name=value".
     Name is required and value is optional. For example, -t tag1=value1;tag2
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb list [options] <resource-group>
<span data-ttu-id="d0f9e-202">Visar belastning belastningsutjämnaren resurser inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-202">Lists Load balancer resources within a resource group.</span></span>

    azure network lb list myresourcegroup

    info:    Executing command network lb list
    + Getting hello load balancers
    data:    Name  Location
    data:    ----  --------
    data:    mylb  westus
    info:    network lb list command OK

<span data-ttu-id="d0f9e-203">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-203">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb show [options] <resource-group> <name>

<span data-ttu-id="d0f9e-204">Visar att läsa in information om belastningsutjämning för en särskild belastningsutjämnare inom en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d0f9e-204">Displays load balancer information of a specific load balancer within a resource group</span></span>

    azure network lb show myresourcegroup mylb -v

    info:    Executing command network lb show
    verbose: Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb show command OK

<span data-ttu-id="d0f9e-205">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-205">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb delete [options] <resource-group> <name>

<span data-ttu-id="d0f9e-206">Ta bort belastningen belastningsutjämnaren resurser.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-206">Delete load balancer resources.</span></span>

    azure network lb delete  myresourcegroup mylb

    info:    Executing command network lb delete
    + Looking up hello load balancer "mylb"
    Delete load balancer "mylb"? [y/n] y
    + Deleting load balancer "mylb"
    info:    network lb delete command OK

<span data-ttu-id="d0f9e-207">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-207">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -n, --name <name>                      hello name of hello load balancer
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="d0f9e-208">**Kommandon toomanage avsökningar av en belastningsutjämnare**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-208">**Commands toomanage probes of a load balancer**</span></span>

    network lb probe create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="d0f9e-209">Skapa hello avsökningen konfiguration för hälsostatus i hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-209">Create hello probe configuration for health status in hello load balancer.</span></span> <span data-ttu-id="d0f9e-210">Kom ihåg toorun kommandot, din belastningsutjämnare kräver en frontend-ip-resurs (utcheckningen kommandot ”azure-nätverk klientdels-ip” tooassign en IP-adress tooload belastningsutjämnare).</span><span class="sxs-lookup"><span data-stu-id="d0f9e-210">Keep in mind toorun this command, your load balancer requires a frontend-ip resource (Check out command "azure network frontend-ip" tooassign an ip address tooload balancer).</span></span>

    azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

    info:    Executing command network lb probe create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe create command OK

<span data-ttu-id="d0f9e-211">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-211">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello probe
    -p, --protocol <protocol>              hello probe protocol
    -o, --port <port>                      hello probe port
    -f, --path <path>                      hello probe path
    -i, --interval <interval>              hello probe interval in seconds
    -c, --count <count>                    hello number of probes
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb probe set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="d0f9e-212">Uppdaterar en befintlig belastningsutjämningsavsökning med nya värden för den.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-212">Updates an existing load balancer probe with new values for it.</span></span>

    azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

    info:    Executing command network lb probe set
        + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe set command OK

<span data-ttu-id="d0f9e-213">Parameteralternativ för</span><span class="sxs-lookup"><span data-stu-id="d0f9e-213">Parameter options</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello probe
    -e, --new-probe-name <new-probe-name>  hello new name of hello probe
    -p, --protocol <protocol>              hello new value for probe protocol
    -o, --port <port>                      hello new value for probe port
    -f, --path <path>                      hello new value for probe path
    -i, --interval <interval>              hello new value for probe interval in seconds
    -c, --count <count>                    hello new value for number of probes
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb probe list [options] <resource-group> <lb-name>

<span data-ttu-id="d0f9e-214">Hello avsökningen listegenskaper för belastningsutjämnaren belastningen.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-214">List hello probe properties for a load balancer set.</span></span>

    C:\>azure network lb probe list -g myresourcegroup -l mylb

    info:    Executing command network lb probe list
    + Looking up hello load balancer "mylb"
    data:    Name       Protocol  Port  Path  Interval  Count
    data:    ---------  --------  ----  ----  --------  -----
    data:    mylbprobe  Tcp       443         300       2
    info:    network lb probe list command OK

<span data-ttu-id="d0f9e-215">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-215">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier


    network lb probe delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="d0f9e-216">Tar bort hello-avsökningen som skapats för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-216">Removes hello probe created for hello load balancer.</span></span>

    azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

    info:    Executing command network lb probe delete
    + Looking up hello load balancer "mylb"
    Delete a probe "mylbprobe?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb probe delete command OK

<span data-ttu-id="d0f9e-217">**Kommandon toomanage klientdelens ip-konfigurationer för en belastningsutjämnare**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-217">**Commands toomanage frontend ip configurations of a load balancer**</span></span>

    network lb frontend-ip create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="d0f9e-218">Skapar klientdelens IP-konfiguration tooan befintliga belastningen belastningsutjämnaren uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-218">Creates a frontend IP configuration tooan existing load balancer set.</span></span>

    azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

    info:    Executing command network lb frontend-ip create
    + Looking up hello load balancer "mylb"
    + Looking up hello subnet "subnet"
    + Creating frontend IP configuration "myfrontendip"
    + Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/Myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    /frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:           10.0.1.4
    data:    Subnet:                       id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Public IP address:
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip create command OK

<BR>

    network lb frontend-ip set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="d0f9e-219">Uppdateringar en befintlig konfiguration av en klientdel IP.hello kommandot nedan lägger till en offentlig IP-adress som kallas mypubip5 tooan befintliga belastningen belastningsutjämnarklientdel IP med namnet myfrontendip.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-219">Updates an existing configuration of a frontend IP.hello command below adds a public IP called mypubip5 tooan existing load balancer frontend IP named myfrontendip.</span></span>

    azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

    info:    Executing command network lb frontend-ip set
    + Looking up hello load balancer "mylb"
    + Looking up hello public ip "mypubip5"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:
    data:    Subnet:
    data:    Public IP address:            id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypubip5
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip set command OK

<span data-ttu-id="d0f9e-220">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-220">Parameter options:</span></span>

    -h, --help                                                         output usage information
    -v, --verbose                                                      use verbose output
    --json                                                             use json output
    -g, --resource-group <resource-group>                              hello name of hello resource group
    -l, --lb-name <lb-name>                                            hello name of hello load balancer
    -n, --name <name>                                                  hello name of hello frontend ip configuration
    -a, --private-ip-address <private-ip-address>                      hello private ip address
    -o, --private-ip-allocation-method <private-ip-allocation-method>  hello private ip allocation method [Static, Dynamic]
    -u, --public-ip-id <public-ip-id>                                  hello public ip identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -i, --public-ip-name <public-ip-name>                              hello public ip name.
    This public ip must exist in hello same resource group as hello lb.
    Please use public-ip-id if that is not hello case.
    -b, --subnet-id <subnet-id>                                        hello subnet id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
    -e, --subnet-name <subnet-name>                                    hello subnet name
    -m, --vnet-name <vnet-name>                                        hello virtual network name.
    This virtual network must exist in hello same resource group as hello lb.
    Please use subnet-id if that is not hello case.
    -s, --subscription <subscription>                                  hello subscription identifier

<BR>

    network lb frontend-ip list [options] <resource-group> <lb-name>

<span data-ttu-id="d0f9e-221">Visar alla hello klientdel IP-resurser som konfigurerats för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-221">Lists all hello frontend IP resources configured for hello load balancer.</span></span>

    azure network lb frontend-ip list -g myresourcegroup -l mylb

    info:    Executing command network lb frontend-ip list
    + Looking up hello load balancer "mylb"
    data:    Name         Provisioning state  Private IP allocation method  Subnet
    data:    -----------  ------------------  ----------------------------  ------
    data:    myprivateip  Succeeded           Dynamic
    info:    network lb frontend-ip list command OK

<span data-ttu-id="d0f9e-222">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-222">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="d0f9e-223">Tar bort hello klientdelens IP-objektet som är associerat tooload belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="d0f9e-223">Deletes hello frontend IP object associated tooload balancer</span></span>

    network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
    info:    Executing command network lb frontend-ip delete
    + Looking up hello load balancer "mylb"
    Delete frontend ip configuration "myfrontendip"? [y/n] y
    + Updating load balancer "mylb"

<span data-ttu-id="d0f9e-224">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-224">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello frontend ip configuration
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="d0f9e-225">**Kommandon toomanage serverdelsadresspooler för belastningsutjämning**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-225">**Commands toomanage backend address pools of a load balancer**</span></span>

    network lb address-pool create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="d0f9e-226">Skapa en backend-adresspool för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-226">Create a backend address pool for a load balancer.</span></span>

    azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

    info:    Executing command network lb address-pool create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
    data:    Name:                      myaddresspool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool create command OK

<span data-ttu-id="d0f9e-227">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-227">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello backend address pool
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb address-pool list [options] <resource-group> <lb-name>

<span data-ttu-id="d0f9e-228">Lista backend IP-adressintervallet för en viss resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d0f9e-228">List backend IP address pool range for a specific resource group</span></span>

    azure network lb address-pool list -g myresourcegroup -l mylb

    info:    Executing command network lb address-pool list
    + Looking up hello load balancer "mylb"
    data:    Name           Provisioning state
    data:    -------------  ------------------
    data:    mybackendpool  Succeeded
    info:    network lb address-pool list command OK

<span data-ttu-id="d0f9e-229">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-229">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -l, --lb-name <lb-name>                hello name of hello load balancer
     -s, --subscription <subscription>      hello subscription identifier

<BR>
    <span data-ttu-id="d0f9e-230">lb-nätverksadresspool ta bort [alternativ] < resursgrupp >< lb-name ><name></span><span class="sxs-lookup"><span data-stu-id="d0f9e-230">network lb address-pool delete [options] <resource-group> <lb-name> <name></span></span>

<span data-ttu-id="d0f9e-231">Tar bort hello backend IP-adresspool intervallet resursen från belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-231">Removes hello backend IP pool range resource from load balancer.</span></span>

    azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

    info:    Executing command network lb address-pool delete
    + Looking up hello load balancer "mylb"
    Delete backend address pool "mybackendpool"? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb address-pool delete command OK

<span data-ttu-id="d0f9e-232">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-232">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello backend address pool
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="d0f9e-233">**Kommandon toomanage regler för inläsning av belastningsutjämnare**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-233">**Commands toomanage load balancer rules**</span></span>

    network lb rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="d0f9e-234">Skapa regler för inläsning av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-234">Create load balancer rules.</span></span>

<span data-ttu-id="d0f9e-235">Du kan skapa en regel för belastningsutjämnare konfigurera hello klientdel slutpunkt för hello belastningsutjämnare och hello backend adresspoolen intervallet tooreceive hello inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-235">You can create a load balancer rule configuring hello frontend endpoint for hello load balancer and hello backend address pool range tooreceive hello incoming network traffic.</span></span> <span data-ttu-id="d0f9e-236">Inställningarna omfattar också hello portar för klientdelens IP-slutpunkt och portar för intervall för hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-236">Settings also include hello ports for frontend IP endpoint and ports for hello backend address pool range.</span></span>

<span data-ttu-id="d0f9e-237">hello följande exempel visas hur toocreate belastningsutjämning regeln hello klientdel slutpunkt som lyssnade tooport 80 TCP och belastningsutjämning nätverkstrafik skickar tooport 8080 för intervall för hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-237">hello following example shows how toocreate a load balancer rule,  hello frontend endpoint listening tooport 80 TCP and load balancing network traffic sending tooport 8080 for hello backend address pool range.</span></span>

    azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


    info:    Executing command network lb rule create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mylbrule
    data:    Name:                      mylbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule create command OK

<BR>

    network lb rule set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="d0f9e-238">Uppdaterar en befintlig regel för belastningsutjämnare i en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-238">Updates an existing load balancer rule set in a specific resource group.</span></span> <span data-ttu-id="d0f9e-239">I följande exempel hello, ändrat vi hello Regelnamn från mylbrule toomynewlbrule.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-239">In hello following example, we changed hello rule name from mylbrule toomynewlbrule.</span></span>

    azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

    info:    Executing command network lb rule set
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mynewlbrule
    data:    Name:                      mynewlbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule set command OK

<span data-ttu-id="d0f9e-240">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-240">Parameter options:</span></span>

    -h, --help                                         output usage information
    -v, --verbose                                      use verbose output
    --json                                             use json output
    -g, --resource-group <resource-group>              hello name of hello resource group
    -l, --lb-name <lb-name>                            hello name of hello load balancer
    -n, --name <name>                                  hello name of hello rule
    -r, --new-rule-name <new-rule-name>                new rule name
    -p, --protocol <protocol>                          hello rule protocol
    -f, --frontend-port <frontend-port>                hello frontend port
    -b, --backend-port <backend-port>                  hello backend port
    -e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
    -i, --idle-timeout <idle-timeout>                  hello idle timeout in minutes
    -a, --probe-name [probe-name]                      hello name of hello probe defined in hello same load balancer
    -t, --frontend-ip-name <frontend-ip-name>          hello name of hello frontend ip configuration in hello same load balancer
    -o, --backend-address-pool <backend-address-pool>  name of hello backend address pool defined in hello same load balancer
    -s, --subscription <subscription>                  hello subscription identifier


    network lb rule list [options] <resource-group> <lb-name>

<span data-ttu-id="d0f9e-241">Visar alla belastningsutjämningsreglerna som konfigurerats för en belastningsutjämnare i en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-241">Lists all load balancer rules configured for a load balancer in a specific resource group.</span></span>

    azure network lb rule list -g myresourcegroup -l mylb

    info:    Executing command network lb rule list
    + Looking up hello load balancer "mylb"
    data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

    data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    info:    network lb rule list command OK

<span data-ttu-id="d0f9e-242">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-242">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier

    network lb rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="d0f9e-243">Tar bort en regel för belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-243">Deletes a load balancer rule.</span></span>

    azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

    info:    Executing command network lb rule delete
    + Looking up hello load balancer "mylb"
    Delete load balancing rule mynewlbrule? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb rule delete command OK

<span data-ttu-id="d0f9e-244">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-244">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="d0f9e-245">**Inkommande NAT-regler för belastningsutjämning för kommandon toomanage**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-245">**Commands toomanage load balancer inbound NAT rules**</span></span>

    network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="d0f9e-246">Skapar en inkommande NAT-regel för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-246">Creates an inbound NAT rule for load balancer.</span></span>

<span data-ttu-id="d0f9e-247">I hello använder följande exempel som vi skapade en NAT-regel från klientdel IP (som definierades tidigare med hjälp av kommandot hello ”azure-nätverk klientdels-ip”) med en lyssningsport för inkommande och utgående port som hello belastningsutjämnare toosend hello-nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-247">In hello following example  we created a NAT rule from frontend IP (which was previously defined using hello "azure network frontend-ip" command) with an inbound listening port and outbound port that hello load balancer uses toosend hello network traffic.</span></span>

    azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              80
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule create command OK

<span data-ttu-id="d0f9e-248">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-248">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          hello name of hello resource group
    -l, --lb-name <lb-name>                        hello name of hello load balancer
    -n, --name <name>                              hello name of hello inbound NAT rule
    -p, --protocol <protocol>                      hello rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            hello frontend port [0-65535]
    -b, --backend-port <backend-port>              hello backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                hello name of hello frontend ip configuration
    -m, --vm-id <vm-id>                            hello VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        hello VM name.This VM must exist in hello same resource group as hello lb.
    Please use vm-id if that is not hello case.
    this parameter will be ignored if --vm-id is specified
    -s, --subscription <subscription>              hello subscription identifier
<BR>

    network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
<span data-ttu-id="d0f9e-249">Uppdaterar en befintlig inkommande nat-regel.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-249">Updates an existing inbound nat rule.</span></span> <span data-ttu-id="d0f9e-250">I följande exempel hello, vi ändrat hello inkommande lyssningsport från 80 too81.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-250">In hello following example, we changed hello inbound listening port from 80 too81.</span></span>

    azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule set
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              81
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule set command OK

<span data-ttu-id="d0f9e-251">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-251">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          hello name of hello resource group
    -l, --lb-name <lb-name>                        hello name of hello load balancer
    -n, --name <name>                              hello name of hello inbound NAT rule
    -p, --protocol <protocol>                      hello rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            hello frontend port [0-65535]
    -b, --backend-port <backend-port>              hello backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                hello name of hello frontend ip configuration
    -m, --vm-id [vm-id]                            hello VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        hello VM name.
    This virtual machine must exist in hello same resource group as hello lb.
    Please use vm-id if that is not hello case
    -s, --subscription <subscription>              hello subscription identifier
<BR>

    network lb inbound-nat-rule list [options] <resource-group> <lb-name>

<span data-ttu-id="d0f9e-252">Visar alla inkommande nat-regler för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-252">Lists all inbound nat rules for load balancer.</span></span>

    azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

    info:    Executing command network lb inbound-nat-rule list
    + Looking up hello load balancer "mylb"
    data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
    data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
    ---------------------
    data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

    info:    network lb inbound-nat-rule list command OK

<span data-ttu-id="d0f9e-253">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-253">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="d0f9e-254">Tar bort NAT-regel för hello belastningsutjämnare i en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-254">Deletes NAT rule for hello load balancer in a specific resource group.</span></span>

    azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

    info:    Executing command network lb inbound-nat-rule delete
    + Looking up hello load balancer "mylb"
    Delete inbound NAT rule "myinboundnat?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb inbound-nat-rule delete command OK

<span data-ttu-id="d0f9e-255">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-255">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello inbound NAT rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="d0f9e-256">**Kommandon toomanage offentliga ip-adresser**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-256">**Commands toomanage public ip addresses**</span></span>

    network public-ip create [options] <resource-group> <name> <location>
<span data-ttu-id="d0f9e-257">Skapar en offentlig ip-resurs.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-257">Creates a public ip resource.</span></span> <span data-ttu-id="d0f9e-258">Du skapar hello offentliga IP-resurs och associera tooa domännamn.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-258">You will create hello public ip resource and associate tooa domain name.</span></span>

    azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
    info:    Executing command network public-ip create
    + Looking up hello public ip "mytestpublicip1"
    + Creating public ip address "mytestpublicip1"
    + Looking up hello public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Dynamic
    data:    Idle timeout:         4
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip create command OK


<span data-ttu-id="d0f9e-259">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-259">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        hello name of hello resource group
    -n, --name <name>                            hello name of hello public ip
    -l, --location <location>                    hello location
    -d, --domain-name-label <domain-name-label>  hello domain name label.
    This set DNS too<domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  hello allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              hello idle timeout in minutes
    -f, --reverse-fqdn <reverse-fqdn>            hello reverse fqdn
    -t, --tags <tags>                            hello list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>            hello subscription identifier
<br>

    network public-ip set [options] <resource-group> <name>
<span data-ttu-id="d0f9e-260">Uppdaterar hello egenskaperna för en befintlig offentlig ip-resurs.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-260">Updates hello properties of an existing public ip resource.</span></span> <span data-ttu-id="d0f9e-261">I följande exempel hello ändrat vi hello offentlig IP-adress från dynamisk tooStatic.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-261">In hello following example we changed hello public IP address from Dynamic tooStatic.</span></span>

    azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
    info:    Executing command network public-ip set
    + Looking up hello public ip "mytestpublicip1"
    + Updating public ip address "mytestpublicip1"
    + Looking up hello public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip set command OK

<span data-ttu-id="d0f9e-262">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-262">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        hello name of hello resource group
    -n, --name <name>                            hello name of hello public ip
    -d, --domain-name-label [domain-name-label]  hello domain name label.
    This set DNS too<domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  hello allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              hello idle timeout in minutes
    -f, --reverse-fqdn [reverse-fqdn]            hello reverse fqdn
    -t, --tags <tags>                            hello list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    --no-tags                                    remove all existing tags
    -s, --subscription <subscription>            hello subscription identifier

<br>
    <span data-ttu-id="d0f9e-263">listan över offentliga ip-nätverk [alternativ] < resursgrupp > listar alla offentliga IP-resurser inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-263">network public-ip list [options] <resource-group> Lists all public IP resources within a resource group.</span></span>

    azure network public-ip list -g myresourcegroup

    info:    Executing command network public-ip list
    + Getting hello public ip addresses
    data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
    data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
    data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
    data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com

<span data-ttu-id="d0f9e-264">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-264">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -s, --subscription <subscription>      hello subscription identifier
<BR>
    <span data-ttu-id="d0f9e-265">nätverket offentliga IP-visa [alternativ] < resursgrupp ><name></span><span class="sxs-lookup"><span data-stu-id="d0f9e-265">network public-ip show [options] <resource-group> <name></span></span>

<span data-ttu-id="d0f9e-266">Visar offentliga ip-egenskaper för en offentlig ip-resurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-266">Displays public ip properties for a public ip resource within a resource group.</span></span>

    azure network public-ip show -g myresourcegroup -n mytestpublicip

    info:    Executing command network public-ip show
    + Looking up hello public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip
    data:    Name:                 mytestpublicip
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip show command OK

<span data-ttu-id="d0f9e-267">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-267">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello public IP
    -s, --subscription <subscription>      hello subscription identifier


    network public-ip delete [options] <resource-group> <name>

<span data-ttu-id="d0f9e-268">Tar bort offentlig ip-resurs.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-268">Deletes public ip resource.</span></span>

    azure network public-ip delete -g group-1 -n mypublicipname
    info:    Executing command network public-ip delete
    + Looking up hello public ip "mypublicipname"
    Delete public ip address "mypublicipname"? [y/n] y
    + Deleting public ip address "mypublicipname"
    info:    network public-ip delete command OK

<span data-ttu-id="d0f9e-269">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-269">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello public IP
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier


<span data-ttu-id="d0f9e-270">**Kommandon toomanage nätverksgränssnitt**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-270">**Commands toomanage network interfaces**</span></span>

    network nic create [options] <resource-group> <name> <location>
<span data-ttu-id="d0f9e-271">Skapar en resurs med namnet nätverksgränssnitt (NIC) som kan användas för belastningsutjämning eller associera tooa virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-271">Creates a resource called network interface (NIC) which can be used for load balancers or associate tooa Virtual Machine.</span></span>

    azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

    info:    Executing command network nic create
    + Looking up hello network interface "testnic1"
    + Looking up hello subnet "subnet-1"
    + Creating network interface "testnic1"
    + Looking up hello network interface "testnic1"
    data:    Id:                     /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/networkInterfaces/testnic1
    data:    Name:                   testnic1
    data:    Type:                   Microsoft.Network/networkInterfaces
    data:    Location:               eastus
    data:    Provisioning state:     Succeeded
    data:    IP configurations:
    data:       Name:                         NIC-config
    data:       Provisioning state:           Succeeded
    data:       Private IP address:           10.0.0.5
    data:       Private IP Allocation Method: Dynamic
    data:       Subnet:                       /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/virtualNetworks/myVNET/subnets/Subnet-1

<span data-ttu-id="d0f9e-272">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="d0f9e-272">Parameter options:</span></span>

    -h, --help                                                       output usage information
    -v, --verbose                                                    use verbose output
    --json                                                           use json output
    -g, --resource-group <resource-group>                            hello name of hello resource group
    -n, --name <name>                                                hello name of hello network interface
    -l, --location <location>                                        hello location
    -w, --network-security-group-id <network-security-group-id>      hello network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
    -o, --network-security-group-name <network-security-group-name>  hello network security group name.
    This network security group must exist in hello same resource group as hello nic.
    Please use network-security-group-id if that is not hello case.
    -i, --public-ip-id <public-ip-id>                                hello public IP identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -p, --public-ip-name <public-ip-name>                            hello public IP name.
    This public ip must exist in hello same resource group as hello nic.
    Please use public-ip-id if that is not hello case.
    -a, --private-ip-address <private-ip-address>                    hello private IP address
    -u, --subnet-id <subnet-id>                                      hello subnet identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
    --subnet-name <subnet-name>                                  hello subnet name
    -m, --subnet-vnet-name <subnet-vnet-name>                        hello vnet name under which subnet-name exists
    -t, --tags <tags>                                                hello comma seperated list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>                                hello subscription identifier
    data:
    info:    network nic create command OK

<BR>

    network nic set [options] <resource-group> <name>

    network nic list [options] <resource-group>
    network nic show [options] <resource-group> <name>
    network nic delete [options] <resource-group> <name>

<span data-ttu-id="d0f9e-273">**Kommandon toomanage nätverkssäkerhetsgrupper**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-273">**Commands toomanage network security groups**</span></span>

    network nsg create [options] <resource-group> <name> <location>
    network nsg set [options] <resource-group> <name>
    network nsg list [options] <resource-group>
    network nsg show [options] <resource-group> <name>
    network nsg delete [options] <resource-group> <name>

<span data-ttu-id="d0f9e-274">**Kommandon toomanage regler för nätverkssäkerhetsgrupper**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-274">**Commands toomanage network security group rules**</span></span>

    network nsg rule create [options] <resource-group> <nsg-name> <name>
    network nsg rule set [options] <resource-group> <nsg-name> <name>
    network nsg rule list [options] <resource-group> <nsg-name>
    network nsg rule show [options] <resource-group> <nsg-name> <name>
    network nsg rule delete [options] <resource-group> <nsg-name> <name>

<span data-ttu-id="d0f9e-275">**Kommandon toomanage trafikhanterarprofilen**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-275">**Commands toomanage traffic manager profile**</span></span>

    network traffic-manager profile create [options] <resource-group> <name>
    network traffic-manager profile set [options] <resource-group> <name>
    network traffic-manager profile list [options] <resource-group>
    network traffic-manager profile show [options] <resource-group> <name>
    network traffic-manager profile delete [options] <resource-group> <name>
    network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>

<span data-ttu-id="d0f9e-276">**Kommandon toomanage traffic manager-slutpunkter**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-276">**Commands toomanage traffic manager endpoints**</span></span>

    network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
    network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
    network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>

<span data-ttu-id="d0f9e-277">**Kommandon toomanage virtuella nätverks-gateway**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-277">**Commands toomanage virtual network gateways**</span></span>

    network gateway list [options] <resource-group>

## <a name="azure-provider-commands-toomanage-resource-provider-registrations"></a><span data-ttu-id="d0f9e-278">Azure-providern: kommandon toomanage resource provider registreringar</span><span class="sxs-lookup"><span data-stu-id="d0f9e-278">azure provider: Commands toomanage resource provider registrations</span></span>
<span data-ttu-id="d0f9e-279">**Visa en lista över providers som är registrerade i Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-279">**List currently registered providers in Resource Manager**</span></span>

    provider list [options]

<span data-ttu-id="d0f9e-280">**Visa information om hello begärt providernamnrymden**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-280">**Show details about hello requested provider namespace**</span></span>

    provider show [options] <namespace>

<span data-ttu-id="d0f9e-281">**Registrera providern med hello prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-281">**Register provider with hello subscription**</span></span>

    provider register [options] <namespace>

<span data-ttu-id="d0f9e-282">**Avregistrera provider med hello prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-282">**Unregister provider with hello subscription**</span></span>

    provider unregister [options] <namespace>

## <a name="azure-resource-commands-toomanage-your-resources"></a><span data-ttu-id="d0f9e-283">Azure-resurs: kommandon toomanage dina resurser</span><span class="sxs-lookup"><span data-stu-id="d0f9e-283">azure resource: Commands toomanage your resources</span></span>
<span data-ttu-id="d0f9e-284">**Skapar en resurs i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-284">**Creates a resource in a resource group**</span></span>

    resource create [options] <resource-group> <name> <resource-type> <location> <api-version>

<span data-ttu-id="d0f9e-285">**Uppdaterar en resurs i en resursgrupp utan parametrar eller mallar**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-285">**Updates a resource in a resource group without any templates or parameters**</span></span>

    resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>

<span data-ttu-id="d0f9e-286">**Visar hello resurser**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-286">**Lists hello resources**</span></span>

    resource list [options] [resource-group]

<span data-ttu-id="d0f9e-287">**Hämtar en resurs i en resursgrupp eller prenumeration**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-287">**Gets one resource within a resource group or subscription**</span></span>

    resource show [options] <resource-group> <name> <resource-type> <api-version>

<span data-ttu-id="d0f9e-288">**Tar bort en resurs i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-288">**Deletes a resource in a resource group**</span></span>

    resource delete [options] <resource-group> <name> <resource-type> <api-version>

## <a name="azure-role-commands-toomanage-your-azure-roles"></a><span data-ttu-id="d0f9e-289">Azure roll: kommandon toomanage dina Azure roller</span><span class="sxs-lookup"><span data-stu-id="d0f9e-289">azure role: Commands toomanage your Azure roles</span></span>
<span data-ttu-id="d0f9e-290">**Hämta alla tillgängliga rolldefinitioner**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-290">**Get all available role definitions**</span></span>

    role list [options]

<span data-ttu-id="d0f9e-291">**Hämta en tillgänglig rolldefinitionen**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-291">**Get an available role definition**</span></span>

    role show [options] [name]

<span data-ttu-id="d0f9e-292">**Kommandon toomanage din rolltilldelning**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-292">**Commands toomanage your role assignment**</span></span>

    role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]

## <a name="azure-storage-commands-toomanage-your-storage-objects"></a><span data-ttu-id="d0f9e-293">Azure storage: kommandon toomanage lagring-objekt</span><span class="sxs-lookup"><span data-stu-id="d0f9e-293">azure storage: Commands toomanage your Storage objects</span></span>
<span data-ttu-id="d0f9e-294">**Kommandon toomanage Storage-konton**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-294">**Commands toomanage your Storage accounts**</span></span>

    storage account list [options]
    storage account show [options] <name>
    storage account create [options] <name>
    storage account set [options] <name>
    storage account delete [options] <name>

<span data-ttu-id="d0f9e-295">**Kommandon toomanage dina nycklar för Lagringskonto**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-295">**Commands toomanage your Storage account keys**</span></span>

    storage account keys list [options] <name>
    storage account keys renew [options] <name>

<span data-ttu-id="d0f9e-296">**Kommandon tooshow anslutningssträngen för lagring**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-296">**Commands tooshow your Storage connection string**</span></span>

    storage account connectionstring show [options] <name>

<span data-ttu-id="d0f9e-297">**Kommandon toomanage behållare för lagring**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-297">**Commands toomanage your Storage containers**</span></span>

    storage container list [options] [prefix]
    storage container show [options] [container]
    storage container create [options] [container]
    storage container delete [options] [container]
    storage container set [options] [container]

<span data-ttu-id="d0f9e-298">**Kommandon toomanage delade åtkomstsignaturer för Storage-behållare**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-298">**Commands toomanage shared access signatures of your Storage container**</span></span>

    storage container sas create [options] [container] [permissions] [expiry]

<span data-ttu-id="d0f9e-299">**Kommandon toomanage lagras åtkomstprinciper för Storage-behållare**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-299">**Commands toomanage stored access policies of your Storage container**</span></span>

    storage container policy create [options] [container] [name]
    storage container policy show [options] [container] [name]
    storage container policy list [options] [container]
    storage container policy set [options] [container] [name]
    storage container policy delete [options] [container] [name]

<span data-ttu-id="d0f9e-300">**Kommandon toomanage Storage-blobbar**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-300">**Commands toomanage your Storage blobs**</span></span>

    storage blob list [options] [container] [prefix]
    storage blob show [options] [container] [blob]
    storage blob delete [options] [container] [blob]
    storage blob upload [options] [file] [container] [blob]
    storage blob download [options] [container] [blob] [destination]

<span data-ttu-id="d0f9e-301">**Kommandon toomanage din blob kopieringsåtgärd**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-301">**Commands toomanage your blob copy operations**</span></span>

    storage blob copy start [options] [sourceUri] [destContainer]
    storage blob copy show [options] [container] [blob]
    storage blob copy stop [options] [container] [blob] [copyid]

<span data-ttu-id="d0f9e-302">**Kommandon toomanage delad åtkomstsignatur för Storage-blob**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-302">**Commands toomanage shared access signature of your Storage blob**</span></span>

    storage blob sas create [options] [container] [blob] [permissions] [expiry]

<span data-ttu-id="d0f9e-303">**Kommandon toomanage din lagringsfilresurser**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-303">**Commands toomanage your Storage file shares**</span></span>

    storage share create [options] [share]
    storage share show [options] [share]
    storage share delete [options] [share]
    storage share list [options] [prefix]

<span data-ttu-id="d0f9e-304">**Kommandon toomanage lagring-filer**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-304">**Commands toomanage your Storage files**</span></span>

    storage file list [options] [share] [path]
    storage file delete [options] [share] [path]
    storage file upload [options] [source] [share] [path]
    storage file download [options] [share] [path] [destination]

<span data-ttu-id="d0f9e-305">**Kommandon toomanage din fil Arkivkatalog**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-305">**Commands toomanage your Storage file directory**</span></span>

    storage directory create [options] [share] [path]
    storage directory delete [options] [share] [path]

<span data-ttu-id="d0f9e-306">**Kommandon toomanage Storage-köer**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-306">**Commands toomanage your Storage queues**</span></span>

    storage queue create [options] [queue]
    storage queue list [options] [prefix]
    storage queue show [options] [queue]
    storage queue delete [options] [queue]

<span data-ttu-id="d0f9e-307">**Kommandon toomanage delade åtkomstsignaturer för Storage-kö**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-307">**Commands toomanage shared access signatures of your Storage queue**</span></span>

    storage queue sas create [options] [queue] [permissions] [expiry]

<span data-ttu-id="d0f9e-308">**Kommandon toomanage lagras åtkomstprinciper för Storage-kö**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-308">**Commands toomanage stored access policies of your Storage queue**</span></span>

    storage queue policy create [options] [queue] [name]
    storage queue policy show [options] [queue] [name]
    storage queue policy list [options] [queue]
    storage queue policy set [options] [queue] [name]
    storage queue policy delete [options] [queue] [name]

<span data-ttu-id="d0f9e-309">**Kommandon toomanage din Loggningsegenskaper för lagring**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-309">**Commands toomanage your Storage logging properties**</span></span>

    storage logging show [options]
    storage logging set [options]

<span data-ttu-id="d0f9e-310">**Kommandon toomanage din mått lagringsegenskaper**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-310">**Commands toomanage your Storage metrics properties**</span></span>

    storage metrics show [options]
    storage metrics set [options]

<span data-ttu-id="d0f9e-311">**Kommandon toomanage Storage-tabeller**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-311">**Commands toomanage your Storage tables**</span></span>

    storage table create [options] [table]
    storage table list [options] [prefix]
    storage table show [options] [table]
    storage table delete [options] [table]

<span data-ttu-id="d0f9e-312">**Kommandon toomanage delade åtkomstsignaturer din tabell för lagring**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-312">**Commands toomanage shared access signatures of your Storage table**</span></span>

    storage table sas create [options] [table] [permissions] [expiry]

<span data-ttu-id="d0f9e-313">**Kommandon toomanage lagras åtkomstprinciper din tabell för lagring**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-313">**Commands toomanage stored access policies of your Storage table**</span></span>

    storage table policy create [options] [table] [name]
    storage table policy show [options] [table] [name]
    storage table policy list [options] [table]
    storage table policy set [options] [table] [name]
    storage table policy delete [options] [table] [name]

## <a name="azure-tag-commands-toomanage-your-resource-manager-tag"></a><span data-ttu-id="d0f9e-314">Azure-taggen: kommandon toomanage din resource manager-tagg</span><span class="sxs-lookup"><span data-stu-id="d0f9e-314">azure tag: Commands toomanage your resource manager tag</span></span>
<span data-ttu-id="d0f9e-315">**Lägga till en tagg**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-315">**Add a tag**</span></span>

    tag create [options] <name> <value>

<span data-ttu-id="d0f9e-316">**Ta bort en hel tagg eller ett Taggvärde**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-316">**Remove an entire tag or a tag value**</span></span>

    tag delete [options] <name> <value>

<span data-ttu-id="d0f9e-317">**Visar hello tagginformation**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-317">**Lists hello tag information**</span></span>

    tag list [options]

<span data-ttu-id="d0f9e-318">**Hämta en tagg**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-318">**Get a tag**</span></span>

    tag show [options] [name]

## <a name="azure-vm-commands-toomanage-your-azure-virtual-machines"></a><span data-ttu-id="d0f9e-319">Azure vm: kommandon toomanage Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="d0f9e-319">azure vm: Commands toomanage your Azure Virtual Machines</span></span>
<span data-ttu-id="d0f9e-320">**Skapa en virtuell dator**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-320">**Create a VM**</span></span>

    vm create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="d0f9e-321">**Skapa en virtuell dator med standardresurser**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-321">**Create a VM with default resources**</span></span>

    vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password

> [!TIP]
> <span data-ttu-id="d0f9e-322">Från och med CLI version 0.10 kan du ange ett kort alias, till exempel ”UbuntuLTS” eller ”Win2012R2Datacenter” som hello `image-urn` för vissa populära Marketplace-bilder.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-322">Starting with CLI version 0.10, you can provide a short alias such as "UbuntuLTS" or "Win2012R2Datacenter" as hello `image-urn` for some popular Marketplace images.</span></span> <span data-ttu-id="d0f9e-323">Kör `azure help vm quick-create` för alternativ.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-323">Run `azure help vm quick-create` for options.</span></span> <span data-ttu-id="d0f9e-324">Dessutom från och med version 0.10, `azure vm quick-create` använder premium-lagring som standard om den är tillgänglig i hello valda regionen.</span><span class="sxs-lookup"><span data-stu-id="d0f9e-324">Additionally, starting with version 0.10, `azure vm quick-create` uses premium storage by default if it's available in hello selected region.</span></span>
> 
> 

<span data-ttu-id="d0f9e-325">**Lista hello virtuella datorer i ett konto**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-325">**List hello virtual machines within an account**</span></span>

    vm list [options]

<span data-ttu-id="d0f9e-326">**Hämta en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-326">**Get one virtual machine within a resource group**</span></span>

    vm show [options] <resource-group> <name>

<span data-ttu-id="d0f9e-327">**Ta bort en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-327">**Delete one virtual machine within a resource group**</span></span>

    vm delete [options] <resource-group> <name>

<span data-ttu-id="d0f9e-328">**Avsluta en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-328">**Shutdown one virtual machine within a resource group**</span></span>

    vm stop [options] <resource-group> <name>

<span data-ttu-id="d0f9e-329">**Starta om en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-329">**Restart one virtual machine within a resource group**</span></span>

    vm restart [options] <resource-group> <name>

<span data-ttu-id="d0f9e-330">**Starta en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-330">**Start one virtual machine within a resource group**</span></span>

    vm start [options] <resource-group> <name>

<span data-ttu-id="d0f9e-331">**Avsluta en virtuell dator i en resurs grupp och versioner hello beräkningsresurser**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-331">**Shutdown one virtual machine within a resource group and releases hello compute resources**</span></span>

    vm deallocate [options] <resource-group> <name>

<span data-ttu-id="d0f9e-332">**Lista över tillgängliga virtuella storlekar**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-332">**List available virtual machine sizes**</span></span>

    vm sizes [options]

<span data-ttu-id="d0f9e-333">**Avbilda hello VM som OS-avbildningen eller VM-avbildning**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-333">**Capture hello VM as OS Image or VM Image**</span></span>

    vm capture [options] <resource-group> <name> <vhd-name-prefix>

<span data-ttu-id="d0f9e-334">**Ange hello tillstånd för hello VM tooGeneralized**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-334">**Set hello state of hello VM tooGeneralized**</span></span>

    vm generalize [options] <resource-group> <name>

<span data-ttu-id="d0f9e-335">**Hämta Instansvy för hello VM**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-335">**Get instance view of hello VM**</span></span>

    vm get-instance-view [options] <resource-group> <name>

<span data-ttu-id="d0f9e-336">**Aktivera tooreset fjärråtkomst till skrivbordet eller SSH-inställningar för en virtuell dator och tooreset hello lösenord för hello-konto som har administratör eller sudo-behörighet**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-336">**Enable you tooreset Remote Desktop Access or SSH settings on a Virtual Machine and tooreset hello password for hello account that has administrator or sudo authority**</span></span>

    vm reset-access [options] <resource-group> <name>

<span data-ttu-id="d0f9e-337">**Uppdatera virtuell dator med nya data**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-337">**Update VM with new data**</span></span>

    vm set [options] <resource-group> <name>

<span data-ttu-id="d0f9e-338">**Kommandon toomanage data virtuella diskar**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-338">**Commands toomanage your Virtual Machine data disks**</span></span>

    vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
    vm disk detach [options] <resource-group> <vm-name> <lun>
    vm disk attach [options] <resource-group> <vm-name> [vhd-url]

<span data-ttu-id="d0f9e-339">**Resurstillägg för kommandon toomanage VM**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-339">**Commands toomanage VM resource extensions**</span></span>

    vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
    vm extension get [options] <resource-group> <vm-name>

<span data-ttu-id="d0f9e-340">**Kommandon toomanage Docker virtuella datorn**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-340">**Commands toomanage your Docker Virtual Machine**</span></span>

    vm docker create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="d0f9e-341">**Kommandon toomanage VM-avbildningar**</span><span class="sxs-lookup"><span data-stu-id="d0f9e-341">**Commands toomanage VM images**</span></span>

    vm image list-publishers [options] <location>
    vm image list-offers [options] <location> <publisher>
    vm image list-skus [options] <location> <publisher> <offer>
    vm image list [options] <location> <publisher> [offer] [sku]
