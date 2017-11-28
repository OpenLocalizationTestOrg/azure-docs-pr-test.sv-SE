---
title: "Azure CLI-kommandona i Resource Manager-läget | Microsoft Docs"
description: "Azure kommandoradsgränssnittet (CLI)-kommandon för att hantera resurser i Resource Manager-distributionsmodellen"
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
ms.openlocfilehash: be957651af78519f678321aec511b71cb18a85f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-commands-in-resource-manager-mode"></a><span data-ttu-id="bfed8-103">Azure CLI-kommandona i Resource Manager-läge</span><span class="sxs-lookup"><span data-stu-id="bfed8-103">Azure CLI commands in Resource Manager mode</span></span>
<span data-ttu-id="bfed8-104">Den här artikeln innehåller syntax och alternativ för Azure-kommandoradsgränssnittet (CLI)-kommandon som du ofta använder för att skapa och hantera Azure-resurser i Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="bfed8-104">This article provides syntax and options for Azure command-line interface (CLI) commands you'd commonly use to create and manage Azure resources in the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="bfed8-105">Du har åtkomst till dessa kommandon genom att köra CLI i Resource Manager (arm)-läge.</span><span class="sxs-lookup"><span data-stu-id="bfed8-105">You access these commands by running the CLI in Resource Manager (arm) mode.</span></span> <span data-ttu-id="bfed8-106">Detta är inte en fullständig referens och CLI-versionen kan indikera att något annorlunda kommandon eller parametrar.</span><span class="sxs-lookup"><span data-stu-id="bfed8-106">This is not a complete reference, and your CLI version may show slightly different commands or parameters.</span></span> <span data-ttu-id="bfed8-107">En allmän översikt över Azure-resurser och resursgrupper finns i [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bfed8-107">For a general overview of Azure resources and resource groups, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="bfed8-108">Detta kallas ibland artikeln visar Resource Manager kommandon läge i Azure CLI, Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="bfed8-108">This article shows Resource Manager mode commands in the Azure CLI, sometimes called Azure CLI 1.0.</span></span> <span data-ttu-id="bfed8-109">Om du vill arbeta i Resource Manager-modellen, du kan också prova den [Azure CLI 2.0](/cli/azure/install-az-cli2), våra nästa generation flera plattformar CLI.</span><span class="sxs-lookup"><span data-stu-id="bfed8-109">To work in the Resource Manager model, you can also try the [Azure CLI 2.0](/cli/azure/install-az-cli2), our next generation multi-platform CLI.</span></span>
><span data-ttu-id="bfed8-110">Lär dig mer om den [gamla och nya Azure CLIs](/cli/azure/old-and-new-clis).</span><span class="sxs-lookup"><span data-stu-id="bfed8-110">Find out more about the [old and new Azure CLIs](/cli/azure/old-and-new-clis).</span></span>
>

<span data-ttu-id="bfed8-111">Starta först [installerar Azure CLI](../cli-install-nodejs.md) och [ansluta till din Azure-prenumeration](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="bfed8-111">To get started, first [install the Azure CLI](../cli-install-nodejs.md) and [connect to your Azure subscription](../xplat-cli-connect.md).</span></span>

<span data-ttu-id="bfed8-112">Skriv den aktuella kommandosyntax och alternativ på kommandoraden i Resource Manager-läget `azure help` eller, för att visa hjälp för ett visst kommando `azure help [command]`.</span><span class="sxs-lookup"><span data-stu-id="bfed8-112">For current command syntax and options at the command line in Resource Manager mode, type `azure help` or, to display help for a specific command, `azure help [command]`.</span></span> <span data-ttu-id="bfed8-113">Även kan hitta CLI exemplen i dokumentationen för att skapa och hantera specifika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bfed8-113">Also find CLI examples in the documentation for creating and managing specific Azure services.</span></span>

<span data-ttu-id="bfed8-114">Valfria parametrar visas inom hakparentes (till exempel `[parameter]`).</span><span class="sxs-lookup"><span data-stu-id="bfed8-114">Optional parameters are shown in square brackets (for example, `[parameter]`).</span></span> <span data-ttu-id="bfed8-115">Alla andra parametrar är obligatoriska.</span><span class="sxs-lookup"><span data-stu-id="bfed8-115">All other parameters are required.</span></span>

<span data-ttu-id="bfed8-116">Förutom kommandot specifika valfria parametrar dokumenteras här, finns det tre valfria parametrar som kan användas för att visa detaljerade utdata, till exempel alternativen och statuskoder.</span><span class="sxs-lookup"><span data-stu-id="bfed8-116">In addition to command-specific optional parameters documented here, there are three optional parameters that can be used to display detailed output such as request options and status codes.</span></span> <span data-ttu-id="bfed8-117">Den `-v` parametern innehåller utförliga data och `-vv` parametern innehåller även detaljerad utförlig utdata.</span><span class="sxs-lookup"><span data-stu-id="bfed8-117">The `-v` parameter provides verbose output, and the `-vv` parameter provides even more detailed verbose output.</span></span> <span data-ttu-id="bfed8-118">Den `--json` alternativet visas resultatet i raw json-format.</span><span class="sxs-lookup"><span data-stu-id="bfed8-118">The `--json` option outputs the result in raw json format.</span></span>

## <a name="setting-the-resource-manager-mode"></a><span data-ttu-id="bfed8-119">Ange Resource Manager-läget</span><span class="sxs-lookup"><span data-stu-id="bfed8-119">Setting the Resource Manager mode</span></span>
<span data-ttu-id="bfed8-120">Använd följande kommando för att aktivera kommandon för Azure CLI Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="bfed8-120">Use the following command to enable Azure CLI Resource Manager mode commands.</span></span>

    azure config mode arm

> [!NOTE]
> <span data-ttu-id="bfed8-121">Av CLI Azure Resource Manager och Azure Service Management-läge kan inte anges samtidigt.</span><span class="sxs-lookup"><span data-stu-id="bfed8-121">The CLI's Azure Resource Manager mode and Azure Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="bfed8-122">Det vill säga kan resurser som skapats i ett läge inte hanteras från andra läge.</span><span class="sxs-lookup"><span data-stu-id="bfed8-122">That is, resources created in one mode cannot be managed from the other mode.</span></span>
> 
> 

## <a name="azure-account-manage-your-account-information"></a><span data-ttu-id="bfed8-123">Azure-konto: hantera din kontoinformation</span><span class="sxs-lookup"><span data-stu-id="bfed8-123">azure account: Manage your account information</span></span>
<span data-ttu-id="bfed8-124">Din Azure-prenumerationsinformationen används av verktyget för att ansluta till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="bfed8-124">Your Azure subscription information is used by the tool to connect to your account.</span></span>

<span data-ttu-id="bfed8-125">**Visa de importera prenumerationerna**</span><span class="sxs-lookup"><span data-stu-id="bfed8-125">**List the imported subscriptions**</span></span>

    account list [options]

<span data-ttu-id="bfed8-126">**Visa information om en prenumeration**</span><span class="sxs-lookup"><span data-stu-id="bfed8-126">**Show details about a subscription**</span></span>  

    account show [options] [subscriptionNameOrId]

<span data-ttu-id="bfed8-127">**Ange den aktuella prenumerationen**</span><span class="sxs-lookup"><span data-stu-id="bfed8-127">**Set the current subscription**</span></span>

    account set [options] <subscriptionNameOrId>

<span data-ttu-id="bfed8-128">**Ta bort en prenumeration eller miljö, eller avmarkera alla lagrad information om kontot och miljö**</span><span class="sxs-lookup"><span data-stu-id="bfed8-128">**Remove a subscription or environment, or clear all of the stored account and environment info**</span></span>  

    account clear [options]

<span data-ttu-id="bfed8-129">**Kommandon för att hantera din miljö för kontot**</span><span class="sxs-lookup"><span data-stu-id="bfed8-129">**Commands to manage your account environment**</span></span>  

    account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]

## <a name="azure-ad-commands-to-display-active-directory-objects"></a><span data-ttu-id="bfed8-130">Azure ad: kommandon för att visa Active Directory-objekt</span><span class="sxs-lookup"><span data-stu-id="bfed8-130">azure ad: Commands to display Active Directory objects</span></span>
<span data-ttu-id="bfed8-131">**Kommandon för att visa active directory-program**</span><span class="sxs-lookup"><span data-stu-id="bfed8-131">**Commands to display active directory applications**</span></span>

    ad app create [options]
    ad app delete [options] <object-id>

<span data-ttu-id="bfed8-132">**Kommandon för att visa active directory-grupper**</span><span class="sxs-lookup"><span data-stu-id="bfed8-132">**Commands to display active directory groups**</span></span>

    ad group list [options]
    ad group show [options]

<span data-ttu-id="bfed8-133">**Kommandon för att ange en active directory sub gruppen eller medlemmen information**</span><span class="sxs-lookup"><span data-stu-id="bfed8-133">**Commands to provide an active directory sub group or member info**</span></span>

    ad group member list [options] [objectId]

<span data-ttu-id="bfed8-134">**Kommandon för att visa active directory-tjänstens huvudnamn**</span><span class="sxs-lookup"><span data-stu-id="bfed8-134">**Commands to display active directory service principals**</span></span>

    ad sp list [options]
    ad sp show [options]
    ad sp create [options] <application-id>
    ad sp delete [options] <object-id>

<span data-ttu-id="bfed8-135">**Kommandon för att visa active directory-användare**</span><span class="sxs-lookup"><span data-stu-id="bfed8-135">**Commands to display active directory users**</span></span>

    ad user list [options]
    ad user show [options]

## <a name="azure-availset-commands-to-manage-your-availability-sets"></a><span data-ttu-id="bfed8-136">Azure availset: kommandon för att hantera din tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="bfed8-136">azure availset: commands to manage your availability sets</span></span>
<span data-ttu-id="bfed8-137">**Skapar en tillgänglighetsuppsättning inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-137">**Creates an availability set within a resource group**</span></span>

    availset create [options] <resource-group> <name> <location> [tags]

<span data-ttu-id="bfed8-138">**Visar en lista över tillgänglighetsuppsättningar inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-138">**Lists the availability sets within a resource group**</span></span>

    availset list [options] <resource-group>

<span data-ttu-id="bfed8-139">**Hämtar en tillgänglighetsuppsättning inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-139">**Gets one availability set within a resource group**</span></span>

    availset show [options] <resource-group> <name>

<span data-ttu-id="bfed8-140">**Tar bort en tillgänglighetsuppsättning inom en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-140">**Deletes one availability set within a resource group**</span></span>

    availset delete [options] <resource-group> <name>

## <a name="azure-config-commands-to-manage-your-local-settings"></a><span data-ttu-id="bfed8-141">Azure config: kommandon för att hantera dina lokala inställningar</span><span class="sxs-lookup"><span data-stu-id="bfed8-141">azure config: commands to manage your local settings</span></span>
<span data-ttu-id="bfed8-142">**Konfigurationsinställningar för listan Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="bfed8-142">**List Azure CLI configuration settings**</span></span>

    config list [options]

<span data-ttu-id="bfed8-143">**Ta bort en konfigurationsinställning**</span><span class="sxs-lookup"><span data-stu-id="bfed8-143">**Delete a config setting**</span></span>

    config delete [options] <name>

<span data-ttu-id="bfed8-144">**Uppdatera en konfigurationsinställning**</span><span class="sxs-lookup"><span data-stu-id="bfed8-144">**Update a config setting**</span></span>

    config set <name> <value>

<span data-ttu-id="bfed8-145">**Anger arbetsläge Azure CLI antingen `arm` eller`asm`**</span><span class="sxs-lookup"><span data-stu-id="bfed8-145">**Sets the Azure CLI working mode to either `arm` or `asm`**</span></span>

    config mode [options] <modename>


## <a name="azure-feature-commands-to-manage-account-features"></a><span data-ttu-id="bfed8-146">Azure-funktionen: kommandon för att hantera kontofunktioner</span><span class="sxs-lookup"><span data-stu-id="bfed8-146">azure feature: commands to manage account features</span></span>
<span data-ttu-id="bfed8-147">**Visa en lista över alla funktioner som är tillgänglig för din prenumeration**</span><span class="sxs-lookup"><span data-stu-id="bfed8-147">**List all features available for your subscription**</span></span>

    feature list [options]

<span data-ttu-id="bfed8-148">**Visar en funktion**</span><span class="sxs-lookup"><span data-stu-id="bfed8-148">**Shows a feature**</span></span>

    feature show [options] <providerName> <featureName>

<span data-ttu-id="bfed8-149">**Registrerar en förhandsgranskade funktion i en resursleverantör**</span><span class="sxs-lookup"><span data-stu-id="bfed8-149">**Registers a previewed feature of a resource provider**</span></span>

    feature register [options] <providerName> <featureName>

## <a name="azure-group-commands-to-manage-your-resource-groups"></a><span data-ttu-id="bfed8-150">Azure-grupp: kommandon för att hantera dina resursgrupper</span><span class="sxs-lookup"><span data-stu-id="bfed8-150">azure group: Commands to manage your resource groups</span></span>
<span data-ttu-id="bfed8-151">**Skapar en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-151">**Creates a resource group**</span></span>

    group create [options] <name> <location>

<span data-ttu-id="bfed8-152">**Ange taggar till en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-152">**Set tags to a resource group**</span></span>

    group set [options] <name> <tags>

<span data-ttu-id="bfed8-153">**Tar bort en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-153">**Deletes a resource group**</span></span>

    group delete [options] <name>

<span data-ttu-id="bfed8-154">**Listar resursgrupper för din prenumeration**</span><span class="sxs-lookup"><span data-stu-id="bfed8-154">**Lists the resource groups for your subscription**</span></span>

    group list [options]

<span data-ttu-id="bfed8-155">**Visar en resursgrupp för din prenumeration**</span><span class="sxs-lookup"><span data-stu-id="bfed8-155">**Shows a resource group for your subscription**</span></span>

    group show [options] <name>

<span data-ttu-id="bfed8-156">**Kommandon för att hantera resurs loggar**</span><span class="sxs-lookup"><span data-stu-id="bfed8-156">**Commands to manage resource group logs**</span></span>

    group log show [options] [name]

<span data-ttu-id="bfed8-157">**Kommandon för att hantera distributionen i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-157">**Commands to manage your deployment in a resource group**</span></span>

    group deployment create [options] [resource-group] [name]
    group deployment list [options] <resource-group> [state]
    group deployment show [options] <resource-group> [deployment-name]
    group deployment stop [options] <resource-group> [deployment-name]

<span data-ttu-id="bfed8-158">**Kommandon för att hantera lokala eller gallery resource grupp mallen**</span><span class="sxs-lookup"><span data-stu-id="bfed8-158">**Commands to manage your local or gallery resource group template**</span></span>

    group template list [options]
    group template show [options] <name>
    group template download [options] [name] [file]
    group template validate [options] <resource-group>

## <a name="azure-hdinsight-commands-to-manage-your-hdinsight-clusters"></a><span data-ttu-id="bfed8-159">Azure hdinsight: kommandon för att hantera dina HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="bfed8-159">azure hdinsight: Commands to manage your HDInsight clusters</span></span>
<span data-ttu-id="bfed8-160">**Kommandon för att skapa eller lägga till en konfigurationsfil för kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-160">**Commands to create or add to a cluster configuration file**</span></span>

    hdinsight config create [options] <configFilePath> <overwrite>
    hdinsight config add-config-values [options] <configFilePath>
    hdinsight config add-script-action [options] <configFilePath>

<span data-ttu-id="bfed8-161">Exempel: Skapa en konfigurationsfil som innehåller en skriptåtgärd ska köras när du skapar ett kluster.</span><span class="sxs-lookup"><span data-stu-id="bfed8-161">Example: Create a configuration file that contains a script action to run when creating a cluster.</span></span>

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

<span data-ttu-id="bfed8-162">**Kommandot för att skapa ett kluster i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-162">**Command to create a cluster in a resource group**</span></span>

    hdinsight cluster create [options] <clusterName>

<span data-ttu-id="bfed8-163">Exempel: Skapa ett Storm på Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="bfed8-163">Example: Create a Storm on Linux cluster</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="bfed8-164">Exempel: Skapa ett kluster med en skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="bfed8-164">Example: Create a cluster with a script action</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="bfed8-165">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-165">Parameter options:</span></span>

    -h, --help                                                 output usage information
    -v, --verbose                                              use verbose output
    -vv                                                        more verbose with debug output
    --json                                                     use json output
    -g --resource-group <resource-group>                       The name of the resource group
    -c, --clusterName <clusterName>                            HDInsight cluster name
    -l, --location <location>                                  Data center location for the cluster
    -y, --osType <osType>                                      HDInsight cluster operating system
    'Windows' or 'Linux'
    --version <version>                                        HDInsight cluster version
    --clusterType <clusterType>                                HDInsight cluster type.
    Hadoop | HBase | Spark | Storm
    --defaultStorageAccountName <storageAccountName>           Storage account url to use for default HDInsight storage
    --defaultStorageAccountKey <storageAccountKey>             Key to the storage account to use for default HDInsight storage
    --defaultStorageContainer <storageContainer>               Container in the storage account to use for HDInsight default storage
    --headNodeSize <headNodeSize>                              (Optional) Head node size for the cluster
    --workerNodeCount <workerNodeCount>                        Number of worker nodes to use for the cluster
    --workerNodeSize <workerNodeSize>                          (Optional) Worker node size for the cluster)
    --zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for the cluster
    --userName <userName>                                      Cluster username
    --password <password>                                      Cluster password
    --sshUserName <sshUserName>                                SSH username (only for Linux clusters)
    --sshPassword <sshPassword>                                SSH password (only for Linux clusters)
    --sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
    --rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
    --rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
    --rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
    For example 12/12/2015 (only for Windows clusters)
    --virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for the cluster.
    Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
    --subnetName <subnetName>                                  (Optional) Subnet for the cluster
    --additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
    Can be multiple.
    In the format of 'accountName#accountKey'.
    For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
    --hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for the external metastore for Hive
    --hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for the external metastore for Hive
    --hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for the external metastore for Hive
    --hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for the external metastore for Hive
    --oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for the external metastore for Oozie
    --oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for the external metastore for Oozie
    --oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for the external metastore for Oozie
    --oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for the external metastore for Oozie
    --configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
    -s, --subscription <id>                                    The subscription id
    --tags <tags>                                              Tags to set to the cluster.
    Can be multiple.
    In the format of 'name=value'.
    Name is required and value is optional.
    For example, --tags tag1=value1;tag2


<span data-ttu-id="bfed8-166">**Kommandot för att ta bort ett kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-166">**Command to delete a cluster**</span></span>

    hdinsight cluster delete [options] <clusterName>

<span data-ttu-id="bfed8-167">**Kommandot för att visa information om kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-167">**Command to show cluster details**</span></span>

    hdinsight cluster show [options] <clusterName>

<span data-ttu-id="bfed8-168">**Kommandot för att visa en lista med alla kluster (i en viss resursgrupp, om angett)**</span><span class="sxs-lookup"><span data-stu-id="bfed8-168">**Command to list all clusters (in a specific resource group, if provided)**</span></span>

    hdinsight cluster list [options]

<span data-ttu-id="bfed8-169">**Kommandot för att ändra storlek på ett kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-169">**Command to resize a cluster**</span></span>

    hdinsight cluster resize [options] <clusterName> <targetInstanceCount>

<span data-ttu-id="bfed8-170">**Kommando för att aktivera HTTP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-170">**Command to enable HTTP access for a cluster**</span></span>

    hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>

<span data-ttu-id="bfed8-171">**Kommando för att inaktivera HTTP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-171">**Command to disable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-http-access [options] <clusterName>

<span data-ttu-id="bfed8-172">**Kommando för att aktivera RDP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-172">**Command to enable RDP access for a cluster**</span></span>

    hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>

<span data-ttu-id="bfed8-173">**Kommando för att inaktivera HTTP-åtkomst för ett kluster**</span><span class="sxs-lookup"><span data-stu-id="bfed8-173">**Command to disable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-rdp-access [options] <clusterName>

## <a name="azure-insights-commands-related-to-monitoring-insights-events-alert-rules-autoscale-settings-metrics"></a><span data-ttu-id="bfed8-174">Azure insikter: kommandon som rör övervakning insikter (händelser, Varningsregler, Autoskala inställningar, mått)</span><span class="sxs-lookup"><span data-stu-id="bfed8-174">azure insights: Commands related to monitoring Insights (events, alert rules, autoscale settings, metrics)</span></span>
<span data-ttu-id="bfed8-175">**Hämta åtgärdsloggar för en prenumeration, en correlationId, en resursgrupp, resurs eller resursprovidern**</span><span class="sxs-lookup"><span data-stu-id="bfed8-175">**Retrieve operation logs for a subscription, a correlationId, a resource group, resource, or resource provider**</span></span>

    insights logs list [options]

## <a name="azure-location-commands-to-get-the-available-locations-for-all-resource-types"></a><span data-ttu-id="bfed8-176">Azure-plats: kommandon för att hämta tillgängliga platser för alla typer av resurser</span><span class="sxs-lookup"><span data-stu-id="bfed8-176">azure location: Commands to get the available locations for all resource types</span></span>
<span data-ttu-id="bfed8-177">**Visa en lista över tillgängliga platser**</span><span class="sxs-lookup"><span data-stu-id="bfed8-177">**List the available locations**</span></span>

    location list [options]

## <a name="azure-network-commands-to-manage-network-resources"></a><span data-ttu-id="bfed8-178">Azure-nätverk: kommandon för att hantera nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="bfed8-178">azure network: Commands to manage network resources</span></span>
<span data-ttu-id="bfed8-179">**Kommandon för att hantera virtuella nätverk**</span><span class="sxs-lookup"><span data-stu-id="bfed8-179">**Commands to manage virtual networks**</span></span>

    network vnet create [options] <resource-group> <name> <location>
<span data-ttu-id="bfed8-180">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="bfed8-180">Creates a virtual network.</span></span> <span data-ttu-id="bfed8-181">I följande exempel skapar vi ett virtuellt nätverk med namnet newvnet för resursen grupp myresourcegroup i USA, västra region.</span><span class="sxs-lookup"><span data-stu-id="bfed8-181">In the following example we create a virtual network named newvnet for resource group myresourcegroup in the West US region.</span></span>

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


<span data-ttu-id="bfed8-182">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-182">Parameter options:</span></span>

     -h, --help                                 output usage information
     -v, --verbose                              use verbose output
    --json                                     use json output
     -g, --resource-group <resource-group>      the name of the resource group
     -n, --name <name>                          the name of the virtual network
     -l, --location <location>                  the location
     -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network
      For example -a 10.0.0.0/24,10.0.1.0/24.
      Default value is 10.0.0.0/8

    -d, --dns-servers <dns-servers>            the comma separated list of DNS servers IP addresses
     -t, --tags <tags>                          the tags set on this virtual network.
      Can be multiple. In the format of "name=value".
      Name is required and value is optional.
      For example, -t tag1=value1;tag2
     -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet set [options] <resource-group> <name>

<span data-ttu-id="bfed8-183">Uppdaterar en konfiguration av virtuellt nätverk inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-183">Updates a virtual network configuration within a resource group.</span></span>

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

<span data-ttu-id="bfed8-184">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-184">Parameter options:</span></span>

       -h, --help                                 output usage information
       -v, --verbose                              use verbose output
       --json                                     use json output
       -g, --resource-group <resource-group>      the name of the resource group
       -n, --name <name>                          the name of the virtual network
       -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network.
        For example -a 10.0.0.0/24,10.0.1.0/24.
        This list will be appended to the current list of address prefixes.
        The address prefixes in this list should not overlap between them.
        The address prefixes in this list should not overlap with existing address prefixes in the vnet.

       -d, --dns-servers [dns-servers]            the comma separated list of DNS servers IP addresses.
        This list will be appended to the current list of DNS server IP addresses.

       -t, --tags <tags>                          the tags set on this virtual network.
        Can be multiple. In the format of "name=value".
        Name is required and value is optional. For example, -t tag1=value1;tag2.
        This list will be appended to the current list of tags

       --no-tags                                  remove all existing tags
       -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet list [options] <resource-group>

<span data-ttu-id="bfed8-185">Kommandot listar alla virtuella nätverk i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-185">The command lists all virtual networks in a resource group.</span></span>

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

<span data-ttu-id="bfed8-186">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-186">Parameter options:</span></span>

      -h, --help                             output usage information
      -v, --verbose                          use verbose output
      --json                                 use json output
      -g, --resource-group <resource-group>  the name of the resource group
      -s, --subscription <subscription>      the subscription identifier

<BR>

    network vnet show [options] <resource-group> <name>
<span data-ttu-id="bfed8-187">Kommandot visar egenskaper för virtuellt nätverk i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-187">The command shows the virtual network properties in a resource group.</span></span>

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
<span data-ttu-id="bfed8-188">Kommandot tar bort ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="bfed8-188">The command removes a virtual network.</span></span>

    azure network vnet delete myresourcegroup newvnetX

    info:    Executing command network vnet delete
    + Looking up virtual network "newvnetX"
    Delete virtual network newvnetX? [y/n] y
    + Deleting virtual network "newvnetX"
    info:    network vnet delete command OK

<span data-ttu-id="bfed8-189">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-189">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -n, --name <name>                      the name of the virtual network
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      the subscription identifier


<span data-ttu-id="bfed8-190">**Kommandon för att hantera virtuella nätverk undernät**</span><span class="sxs-lookup"><span data-stu-id="bfed8-190">**Commands to manage virtual network subnets**</span></span>

    network vnet subnet create [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="bfed8-191">Lägger till ett annat undernät i ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="bfed8-191">Adds another subnet to an existing virtual network.</span></span>

    azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

    info:    Executing command network vnet subnet create
    + Looking up the subnet "subnet"
    + Creating subnet "subnet"
    + Looking up the subnet "subnet"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Name:                      subnet
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet create command OK

<span data-ttu-id="bfed8-192">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-192">Parameter options:</span></span>

     -h, --help                                                       output usage information
     -v, --verbose                                                    use verbose output
         --json                                                           use json output
     -g, --resource-group <resource-group>                            the name of the resource group
     -e, --vnet-name <vnet-name>                                      the name of the virtual network
     -n, --name <name>                                                the name of the subnet
     -a, --address-prefix <address-prefix>                            the address prefix
     -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
           e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
     -o, --network-security-group-name <network-security-group-name>  the network security group name
     -s, --subscription <subscription>                                the subscription identifier

<BR>

    network vnet subnet set [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="bfed8-193">Anger ett specifikt virtuellt undernät inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-193">Sets a specific virtual network subnet within a resource group.</span></span>

    C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet list [options] <resource-group> <vnet-name>

<span data-ttu-id="bfed8-194">Visar en lista över alla undernät för virtuellt nätverk för ett virtuellt nätverk inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-194">Lists all the virtual network subnets for a specific virtual network within a resource group.</span></span>

    azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet show [options] <resource-group> <vnet-name> <name>
<span data-ttu-id="bfed8-195">Visar egenskaper för undernät för virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="bfed8-195">Displays virtual network subnet properties</span></span>

    azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet show
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
    .Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet show command OK

<span data-ttu-id="bfed8-196">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-196">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -e, --vnet-name <vnet-name>            the name of the virtual network
    -n, --name <name>                      the name of the subnet
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
<span data-ttu-id="bfed8-197">Tar bort ett undernät från ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="bfed8-197">Removes a subnet from an existing virtual network.</span></span>

    azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet delete
    + Looking up the subnet "subnet1"
    Delete subnet "subnet1"? [y/n] y
    + Deleting subnet "subnet1"
    info:    network vnet subnet delete command OK

<span data-ttu-id="bfed8-198">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-198">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -e, --vnet-name <vnet-name>            the name of the virtual network
     -n, --name <name>                      the subnet name
     -s, --subscription <subscription>      the subscription identifier
     -q, --quiet                            quiet mode, do not ask for delete confirmation

<span data-ttu-id="bfed8-199">**Kommandon för att hantera belastningsutjämnare**</span><span class="sxs-lookup"><span data-stu-id="bfed8-199">**Commands to manage load balancers**</span></span>

    network lb create [options] <resource-group> <name> <location>
<span data-ttu-id="bfed8-200">Skapar en belastningen belastningsutjämnaren mängd.</span><span class="sxs-lookup"><span data-stu-id="bfed8-200">Creates a load balancer set.</span></span>

    azure network lb create -g myresourcegroup -n mylb -l westus

    info:    Executing command network lb create
    + Looking up the load balancer "mylb"
    + Creating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb create command OK

<span data-ttu-id="bfed8-201">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-201">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -l, --location <location>              the location
    -t, --tags <tags>                      the list of tags.
     Can be multiple. In the format of "name=value".
     Name is required and value is optional. For example, -t tag1=value1;tag2
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb list [options] <resource-group>
<span data-ttu-id="bfed8-202">Visar belastning belastningsutjämnaren resurser inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-202">Lists Load balancer resources within a resource group.</span></span>

    azure network lb list myresourcegroup

    info:    Executing command network lb list
    + Getting the load balancers
    data:    Name  Location
    data:    ----  --------
    data:    mylb  westus
    info:    network lb list command OK

<span data-ttu-id="bfed8-203">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-203">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb show [options] <resource-group> <name>

<span data-ttu-id="bfed8-204">Visar att läsa in information om belastningsutjämning för en särskild belastningsutjämnare inom en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="bfed8-204">Displays load balancer information of a specific load balancer within a resource group</span></span>

    azure network lb show myresourcegroup mylb -v

    info:    Executing command network lb show
    verbose: Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb show command OK

<span data-ttu-id="bfed8-205">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-205">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb delete [options] <resource-group> <name>

<span data-ttu-id="bfed8-206">Ta bort belastningen belastningsutjämnaren resurser.</span><span class="sxs-lookup"><span data-stu-id="bfed8-206">Delete load balancer resources.</span></span>

    azure network lb delete  myresourcegroup mylb

    info:    Executing command network lb delete
    + Looking up the load balancer "mylb"
    Delete load balancer "mylb"? [y/n] y
    + Deleting load balancer "mylb"
    info:    network lb delete command OK

<span data-ttu-id="bfed8-207">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-207">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -n, --name <name>                      the name of the load balancer
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="bfed8-208">**Kommandon för att hantera avsökningar av en belastningsutjämnare**</span><span class="sxs-lookup"><span data-stu-id="bfed8-208">**Commands to manage probes of a load balancer**</span></span>

    network lb probe create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="bfed8-209">Skapa avsökningen konfigurationen för hälsostatus i belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bfed8-209">Create the probe configuration for health status in the load balancer.</span></span> <span data-ttu-id="bfed8-210">Tänk på att köra det här kommandot, din belastningsutjämnare kräver en frontend-ip-resurs (checka ut ”azure-nätverk klientdels-ip” att tilldela en ip-adress till belastningsutjämnaren).</span><span class="sxs-lookup"><span data-stu-id="bfed8-210">Keep in mind to run this command, your load balancer requires a frontend-ip resource (Check out command "azure network frontend-ip" to assign an ip address to load balancer).</span></span>

    azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

    info:    Executing command network lb probe create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe create command OK

<span data-ttu-id="bfed8-211">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-211">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -p, --protocol <protocol>              the probe protocol
    -o, --port <port>                      the probe port
    -f, --path <path>                      the probe path
    -i, --interval <interval>              the probe interval in seconds
    -c, --count <count>                    the number of probes
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb probe set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="bfed8-212">Uppdaterar en befintlig belastningsutjämningsavsökning med nya värden för den.</span><span class="sxs-lookup"><span data-stu-id="bfed8-212">Updates an existing load balancer probe with new values for it.</span></span>

    azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

    info:    Executing command network lb probe set
        + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe set command OK

<span data-ttu-id="bfed8-213">Parameteralternativ för</span><span class="sxs-lookup"><span data-stu-id="bfed8-213">Parameter options</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -e, --new-probe-name <new-probe-name>  the new name of the probe
    -p, --protocol <protocol>              the new value for probe protocol
    -o, --port <port>                      the new value for probe port
    -f, --path <path>                      the new value for probe path
    -i, --interval <interval>              the new value for probe interval in seconds
    -c, --count <count>                    the new value for number of probes
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb probe list [options] <resource-group> <lb-name>

<span data-ttu-id="bfed8-214">Visa avsökning egenskaperna för belastningsutjämnaren belastningen.</span><span class="sxs-lookup"><span data-stu-id="bfed8-214">List the probe properties for a load balancer set.</span></span>

    C:\>azure network lb probe list -g myresourcegroup -l mylb

    info:    Executing command network lb probe list
    + Looking up the load balancer "mylb"
    data:    Name       Protocol  Port  Path  Interval  Count
    data:    ---------  --------  ----  ----  --------  -----
    data:    mylbprobe  Tcp       443         300       2
    info:    network lb probe list command OK

<span data-ttu-id="bfed8-215">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-215">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier


    network lb probe delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="bfed8-216">Tar bort avsökningen skapas för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bfed8-216">Removes the probe created for the load balancer.</span></span>

    azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

    info:    Executing command network lb probe delete
    + Looking up the load balancer "mylb"
    Delete a probe "mylbprobe?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb probe delete command OK

<span data-ttu-id="bfed8-217">**Kommandon för att hantera klientdelens ip-konfigurationer för en belastningsutjämnare**</span><span class="sxs-lookup"><span data-stu-id="bfed8-217">**Commands to manage frontend ip configurations of a load balancer**</span></span>

    network lb frontend-ip create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="bfed8-218">Skapar en klientdel IP-konfiguration till en befintlig load balancer uppsättning.</span><span class="sxs-lookup"><span data-stu-id="bfed8-218">Creates a frontend IP configuration to an existing load balancer set.</span></span>

    azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

    info:    Executing command network lb frontend-ip create
    + Looking up the load balancer "mylb"
    + Looking up the subnet "subnet"
    + Creating frontend IP configuration "myfrontendip"
    + Looking up the load balancer "mylb"
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

<span data-ttu-id="bfed8-219">Uppdaterar en befintlig konfiguration av en klientdel IP-adress. Kommandot nedan lägger till en offentlig IP-adress som kallas mypubip5 till en befintlig belastningen belastningsutjämnaren klientdelens IP-med namnet myfrontendip.</span><span class="sxs-lookup"><span data-stu-id="bfed8-219">Updates an existing configuration of a frontend IP.The command below adds a public IP called mypubip5 to an existing load balancer frontend IP named myfrontendip.</span></span>

    azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

    info:    Executing command network lb frontend-ip set
    + Looking up the load balancer "mylb"
    + Looking up the public ip "mypubip5"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
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

<span data-ttu-id="bfed8-220">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-220">Parameter options:</span></span>

    -h, --help                                                         output usage information
    -v, --verbose                                                      use verbose output
    --json                                                             use json output
    -g, --resource-group <resource-group>                              the name of the resource group
    -l, --lb-name <lb-name>                                            the name of the load balancer
    -n, --name <name>                                                  the name of the frontend ip configuration
    -a, --private-ip-address <private-ip-address>                      the private ip address
    -o, --private-ip-allocation-method <private-ip-allocation-method>  the private ip allocation method [Static, Dynamic]
    -u, --public-ip-id <public-ip-id>                                  the public ip identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -i, --public-ip-name <public-ip-name>                              the public ip name.
    This public ip must exist in the same resource group as the lb.
    Please use public-ip-id if that is not the case.
    -b, --subnet-id <subnet-id>                                        the subnet id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
    -e, --subnet-name <subnet-name>                                    the subnet name
    -m, --vnet-name <vnet-name>                                        the virtual network name.
    This virtual network must exist in the same resource group as the lb.
    Please use subnet-id if that is not the case.
    -s, --subscription <subscription>                                  the subscription identifier

<BR>

    network lb frontend-ip list [options] <resource-group> <lb-name>

<span data-ttu-id="bfed8-221">Listar alla klientdelens IP-resurser som konfigurerats för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bfed8-221">Lists all the frontend IP resources configured for the load balancer.</span></span>

    azure network lb frontend-ip list -g myresourcegroup -l mylb

    info:    Executing command network lb frontend-ip list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Private IP allocation method  Subnet
    data:    -----------  ------------------  ----------------------------  ------
    data:    myprivateip  Succeeded           Dynamic
    info:    network lb frontend-ip list command OK

<span data-ttu-id="bfed8-222">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-222">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="bfed8-223">Tar bort klientdelens IP-objekt som behövs för att belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="bfed8-223">Deletes the frontend IP object associated to load balancer</span></span>

    network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
    info:    Executing command network lb frontend-ip delete
    + Looking up the load balancer "mylb"
    Delete frontend ip configuration "myfrontendip"? [y/n] y
    + Updating load balancer "mylb"

<span data-ttu-id="bfed8-224">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-224">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the frontend ip configuration
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="bfed8-225">**Kommandon för att hantera serverdelsadresspooler för belastningsutjämning**</span><span class="sxs-lookup"><span data-stu-id="bfed8-225">**Commands to manage backend address pools of a load balancer**</span></span>

    network lb address-pool create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="bfed8-226">Skapa en backend-adresspool för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="bfed8-226">Create a backend address pool for a load balancer.</span></span>

    azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

    info:    Executing command network lb address-pool create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
    data:    Name:                      myaddresspool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool create command OK

<span data-ttu-id="bfed8-227">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-227">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb address-pool list [options] <resource-group> <lb-name>

<span data-ttu-id="bfed8-228">Lista backend IP-adressintervallet för en viss resursgrupp</span><span class="sxs-lookup"><span data-stu-id="bfed8-228">List backend IP address pool range for a specific resource group</span></span>

    azure network lb address-pool list -g myresourcegroup -l mylb

    info:    Executing command network lb address-pool list
    + Looking up the load balancer "mylb"
    data:    Name           Provisioning state
    data:    -------------  ------------------
    data:    mybackendpool  Succeeded
    info:    network lb address-pool list command OK

<span data-ttu-id="bfed8-229">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-229">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -l, --lb-name <lb-name>                the name of the load balancer
     -s, --subscription <subscription>      the subscription identifier

<BR>
    <span data-ttu-id="bfed8-230">lb-nätverksadresspool ta bort [alternativ] < resursgrupp >< lb-name ><name></span><span class="sxs-lookup"><span data-stu-id="bfed8-230">network lb address-pool delete [options] <resource-group> <lb-name> <name></span></span>

<span data-ttu-id="bfed8-231">Tar bort backend IP-adresspool intervallet resursen från belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bfed8-231">Removes the backend IP pool range resource from load balancer.</span></span>

    azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

    info:    Executing command network lb address-pool delete
    + Looking up the load balancer "mylb"
    Delete backend address pool "mybackendpool"? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb address-pool delete command OK

<span data-ttu-id="bfed8-232">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-232">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="bfed8-233">**Kommandon för att hantera belastningsutjämningsreglerna**</span><span class="sxs-lookup"><span data-stu-id="bfed8-233">**Commands to manage load balancer rules**</span></span>

    network lb rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="bfed8-234">Skapa regler för inläsning av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="bfed8-234">Create load balancer rules.</span></span>

<span data-ttu-id="bfed8-235">Du kan skapa en regel för belastningsutjämnare Konfigurera frontend-slutpunkten för belastningsutjämnaren och backend-adressintervallet för att ta emot inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="bfed8-235">You can create a load balancer rule configuring the frontend endpoint for the load balancer and the backend address pool range to receive the incoming network traffic.</span></span> <span data-ttu-id="bfed8-236">Inställningarna omfattar också portar för klientdelens IP-slutpunkt och portar för intervall för backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="bfed8-236">Settings also include the ports for frontend IP endpoint and ports for the backend address pool range.</span></span>

<span data-ttu-id="bfed8-237">I följande exempel visas hur du skapar en regel för belastningsutjämnare, klientdel slutpunkten lyssna på port 80 TCP och läsa in belastningsutjämning nätverkstrafiken som skickas till port 8080 för intervall för backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="bfed8-237">The following example shows how to create a load balancer rule,  the frontend endpoint listening to port 80 TCP and load balancing network traffic sending to port 8080 for the backend address pool range.</span></span>

    azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


    info:    Executing command network lb rule create
    + Looking up the load balancer "mylb"
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

<span data-ttu-id="bfed8-238">Uppdaterar en befintlig regel för belastningsutjämnare i en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-238">Updates an existing load balancer rule set in a specific resource group.</span></span> <span data-ttu-id="bfed8-239">I följande exempel ändras vi Regelnamnet från mylbrule till mynewlbrule.</span><span class="sxs-lookup"><span data-stu-id="bfed8-239">In the following example, we changed the rule name from mylbrule to mynewlbrule.</span></span>

    azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

    info:    Executing command network lb rule set
    + Looking up the load balancer "mylb"
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

<span data-ttu-id="bfed8-240">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-240">Parameter options:</span></span>

    -h, --help                                         output usage information
    -v, --verbose                                      use verbose output
    --json                                             use json output
    -g, --resource-group <resource-group>              the name of the resource group
    -l, --lb-name <lb-name>                            the name of the load balancer
    -n, --name <name>                                  the name of the rule
    -r, --new-rule-name <new-rule-name>                new rule name
    -p, --protocol <protocol>                          the rule protocol
    -f, --frontend-port <frontend-port>                the frontend port
    -b, --backend-port <backend-port>                  the backend port
    -e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
    -i, --idle-timeout <idle-timeout>                  the idle timeout in minutes
    -a, --probe-name [probe-name]                      the name of the probe defined in the same load balancer
    -t, --frontend-ip-name <frontend-ip-name>          the name of the frontend ip configuration in the same load balancer
    -o, --backend-address-pool <backend-address-pool>  name of the backend address pool defined in the same load balancer
    -s, --subscription <subscription>                  the subscription identifier


    network lb rule list [options] <resource-group> <lb-name>

<span data-ttu-id="bfed8-241">Visar alla belastningsutjämningsreglerna som konfigurerats för en belastningsutjämnare i en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-241">Lists all load balancer rules configured for a load balancer in a specific resource group.</span></span>

    azure network lb rule list -g myresourcegroup -l mylb

    info:    Executing command network lb rule list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

    data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    info:    network lb rule list command OK

<span data-ttu-id="bfed8-242">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-242">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

    network lb rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="bfed8-243">Tar bort en regel för belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="bfed8-243">Deletes a load balancer rule.</span></span>

    azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

    info:    Executing command network lb rule delete
    + Looking up the load balancer "mylb"
    Delete load balancing rule mynewlbrule? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb rule delete command OK

<span data-ttu-id="bfed8-244">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-244">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="bfed8-245">**Kommandon för att hantera belastningsutjämnaren inkommande NAT-regler**</span><span class="sxs-lookup"><span data-stu-id="bfed8-245">**Commands to manage load balancer inbound NAT rules**</span></span>

    network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="bfed8-246">Skapar en inkommande NAT-regel för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="bfed8-246">Creates an inbound NAT rule for load balancer.</span></span>

<span data-ttu-id="bfed8-247">I följande exempel skapade vi en NAT-regel från klientdel IP (som definierades tidigare med hjälp av kommandot ”azure-nätverk klientdels-ip”) med en lyssningsport för inkommande och utgående port som belastningsutjämnaren skickar nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="bfed8-247">In the following example  we created a NAT rule from frontend IP (which was previously defined using the "azure network frontend-ip" command) with an inbound listening port and outbound port that the load balancer uses to send the network traffic.</span></span>

    azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
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

<span data-ttu-id="bfed8-248">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-248">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id <vm-id>                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.This VM must exist in the same resource group as the lb.
    Please use vm-id if that is not the case.
    this parameter will be ignored if --vm-id is specified
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
<span data-ttu-id="bfed8-249">Uppdaterar en befintlig inkommande nat-regel.</span><span class="sxs-lookup"><span data-stu-id="bfed8-249">Updates an existing inbound nat rule.</span></span> <span data-ttu-id="bfed8-250">I följande exempel ändras vi inkommande lyssningsporten från 80 till 81.</span><span class="sxs-lookup"><span data-stu-id="bfed8-250">In the following example, we changed the inbound listening port from 80 to 81.</span></span>

    azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule set
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
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

<span data-ttu-id="bfed8-251">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-251">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id [vm-id]                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.
    This virtual machine must exist in the same resource group as the lb.
    Please use vm-id if that is not the case
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule list [options] <resource-group> <lb-name>

<span data-ttu-id="bfed8-252">Visar alla inkommande nat-regler för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="bfed8-252">Lists all inbound nat rules for load balancer.</span></span>

    azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

    info:    Executing command network lb inbound-nat-rule list
    + Looking up the load balancer "mylb"
    data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
    data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
    ---------------------
    data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

    info:    network lb inbound-nat-rule list command OK

<span data-ttu-id="bfed8-253">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-253">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="bfed8-254">Tar bort NAT-regel för belastningsutjämnare i en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-254">Deletes NAT rule for the load balancer in a specific resource group.</span></span>

    azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

    info:    Executing command network lb inbound-nat-rule delete
    + Looking up the load balancer "mylb"
    Delete inbound NAT rule "myinboundnat?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb inbound-nat-rule delete command OK

<span data-ttu-id="bfed8-255">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-255">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the inbound NAT rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="bfed8-256">**Kommandon för att hantera offentliga ip-adresser**</span><span class="sxs-lookup"><span data-stu-id="bfed8-256">**Commands to manage public ip addresses**</span></span>

    network public-ip create [options] <resource-group> <name> <location>
<span data-ttu-id="bfed8-257">Skapar en offentlig ip-resurs.</span><span class="sxs-lookup"><span data-stu-id="bfed8-257">Creates a public ip resource.</span></span> <span data-ttu-id="bfed8-258">Du skapar den offentliga ip-resursen och koppla till ett domännamn.</span><span class="sxs-lookup"><span data-stu-id="bfed8-258">You will create the public ip resource and associate to a domain name.</span></span>

    azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
    info:    Executing command network public-ip create
    + Looking up the public ip "mytestpublicip1"
    + Creating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
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


<span data-ttu-id="bfed8-259">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-259">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -l, --location <location>                    the location
    -d, --domain-name-label <domain-name-label>  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn <reverse-fqdn>            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>            the subscription identifier
<br>

    network public-ip set [options] <resource-group> <name>
<span data-ttu-id="bfed8-260">Uppdaterar egenskaperna för en befintlig offentlig ip-resurs.</span><span class="sxs-lookup"><span data-stu-id="bfed8-260">Updates the properties of an existing public ip resource.</span></span> <span data-ttu-id="bfed8-261">I följande exempel ändras vi den offentliga IP-adressen från dynamisk till statisk.</span><span class="sxs-lookup"><span data-stu-id="bfed8-261">In the following example we changed the public IP address from Dynamic to Static.</span></span>

    azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
    info:    Executing command network public-ip set
    + Looking up the public ip "mytestpublicip1"
    + Updating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
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

<span data-ttu-id="bfed8-262">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-262">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -d, --domain-name-label [domain-name-label]  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn [reverse-fqdn]            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    --no-tags                                    remove all existing tags
    -s, --subscription <subscription>            the subscription identifier

<br>
    <span data-ttu-id="bfed8-263">listan över offentliga ip-nätverk [alternativ] < resursgrupp > listar alla offentliga IP-resurser inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-263">network public-ip list [options] <resource-group> Lists all public IP resources within a resource group.</span></span>

    azure network public-ip list -g myresourcegroup

    info:    Executing command network public-ip list
    + Getting the public ip addresses
    data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
    data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
    data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
    data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com

<span data-ttu-id="bfed8-264">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-264">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier
<BR>
    <span data-ttu-id="bfed8-265">nätverket offentliga IP-visa [alternativ] < resursgrupp ><name></span><span class="sxs-lookup"><span data-stu-id="bfed8-265">network public-ip show [options] <resource-group> <name></span></span>

<span data-ttu-id="bfed8-266">Visar offentliga ip-egenskaper för en offentlig ip-resurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bfed8-266">Displays public ip properties for a public ip resource within a resource group.</span></span>

    azure network public-ip show -g myresourcegroup -n mytestpublicip

    info:    Executing command network public-ip show
    + Looking up the public ip "mytestpublicip1"
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

<span data-ttu-id="bfed8-267">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-267">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -s, --subscription <subscription>      the subscription identifier


    network public-ip delete [options] <resource-group> <name>

<span data-ttu-id="bfed8-268">Tar bort offentlig ip-resurs.</span><span class="sxs-lookup"><span data-stu-id="bfed8-268">Deletes public ip resource.</span></span>

    azure network public-ip delete -g group-1 -n mypublicipname
    info:    Executing command network public-ip delete
    + Looking up the public ip "mypublicipname"
    Delete public ip address "mypublicipname"? [y/n] y
    + Deleting public ip address "mypublicipname"
    info:    network public-ip delete command OK

<span data-ttu-id="bfed8-269">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-269">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier


<span data-ttu-id="bfed8-270">**Kommandon för att hantera nätverksgränssnitt**</span><span class="sxs-lookup"><span data-stu-id="bfed8-270">**Commands to manage network interfaces**</span></span>

    network nic create [options] <resource-group> <name> <location>
<span data-ttu-id="bfed8-271">Skapar en resurs med namnet nätverksgränssnitt (NIC) som kan användas för belastningsutjämning eller koppla till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bfed8-271">Creates a resource called network interface (NIC) which can be used for load balancers or associate to a Virtual Machine.</span></span>

    azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

    info:    Executing command network nic create
    + Looking up the network interface "testnic1"
    + Looking up the subnet "subnet-1"
    + Creating network interface "testnic1"
    + Looking up the network interface "testnic1"
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

<span data-ttu-id="bfed8-272">Parameteralternativ:</span><span class="sxs-lookup"><span data-stu-id="bfed8-272">Parameter options:</span></span>

    -h, --help                                                       output usage information
    -v, --verbose                                                    use verbose output
    --json                                                           use json output
    -g, --resource-group <resource-group>                            the name of the resource group
    -n, --name <name>                                                the name of the network interface
    -l, --location <location>                                        the location
    -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
    -o, --network-security-group-name <network-security-group-name>  the network security group name.
    This network security group must exist in the same resource group as the nic.
    Please use network-security-group-id if that is not the case.
    -i, --public-ip-id <public-ip-id>                                the public IP identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -p, --public-ip-name <public-ip-name>                            the public IP name.
    This public ip must exist in the same resource group as the nic.
    Please use public-ip-id if that is not the case.
    -a, --private-ip-address <private-ip-address>                    the private IP address
    -u, --subnet-id <subnet-id>                                      the subnet identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
    --subnet-name <subnet-name>                                  the subnet name
    -m, --subnet-vnet-name <subnet-vnet-name>                        the vnet name under which subnet-name exists
    -t, --tags <tags>                                                the comma seperated list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>                                the subscription identifier
    data:
    info:    network nic create command OK

<BR>

    network nic set [options] <resource-group> <name>

    network nic list [options] <resource-group>
    network nic show [options] <resource-group> <name>
    network nic delete [options] <resource-group> <name>

<span data-ttu-id="bfed8-273">**Kommandon för att hantera nätverkssäkerhetsgrupper**</span><span class="sxs-lookup"><span data-stu-id="bfed8-273">**Commands to manage network security groups**</span></span>

    network nsg create [options] <resource-group> <name> <location>
    network nsg set [options] <resource-group> <name>
    network nsg list [options] <resource-group>
    network nsg show [options] <resource-group> <name>
    network nsg delete [options] <resource-group> <name>

<span data-ttu-id="bfed8-274">**Kommandon för att hantera nätverk regler för nätverkssäkerhetsgrupper**</span><span class="sxs-lookup"><span data-stu-id="bfed8-274">**Commands to manage network security group rules**</span></span>

    network nsg rule create [options] <resource-group> <nsg-name> <name>
    network nsg rule set [options] <resource-group> <nsg-name> <name>
    network nsg rule list [options] <resource-group> <nsg-name>
    network nsg rule show [options] <resource-group> <nsg-name> <name>
    network nsg rule delete [options] <resource-group> <nsg-name> <name>

<span data-ttu-id="bfed8-275">**Kommandon för att hantera trafikhanterarprofilen**</span><span class="sxs-lookup"><span data-stu-id="bfed8-275">**Commands to manage traffic manager profile**</span></span>

    network traffic-manager profile create [options] <resource-group> <name>
    network traffic-manager profile set [options] <resource-group> <name>
    network traffic-manager profile list [options] <resource-group>
    network traffic-manager profile show [options] <resource-group> <name>
    network traffic-manager profile delete [options] <resource-group> <name>
    network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>

<span data-ttu-id="bfed8-276">**Kommandon för att hantera traffic manager-slutpunkter**</span><span class="sxs-lookup"><span data-stu-id="bfed8-276">**Commands to manage traffic manager endpoints**</span></span>

    network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
    network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
    network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>

<span data-ttu-id="bfed8-277">**Kommandon för att hantera virtuella nätverk gateways**</span><span class="sxs-lookup"><span data-stu-id="bfed8-277">**Commands to manage virtual network gateways**</span></span>

    network gateway list [options] <resource-group>

## <a name="azure-provider-commands-to-manage-resource-provider-registrations"></a><span data-ttu-id="bfed8-278">Azure-providern: kommandon för att hantera resource provider registreringar</span><span class="sxs-lookup"><span data-stu-id="bfed8-278">azure provider: Commands to manage resource provider registrations</span></span>
<span data-ttu-id="bfed8-279">**Visa en lista över providers som är registrerade i Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="bfed8-279">**List currently registered providers in Resource Manager**</span></span>

    provider list [options]

<span data-ttu-id="bfed8-280">**Visa information om begärda leverantörens namnrymd**</span><span class="sxs-lookup"><span data-stu-id="bfed8-280">**Show details about the requested provider namespace**</span></span>

    provider show [options] <namespace>

<span data-ttu-id="bfed8-281">**Registrera providern med prenumerationen**</span><span class="sxs-lookup"><span data-stu-id="bfed8-281">**Register provider with the subscription**</span></span>

    provider register [options] <namespace>

<span data-ttu-id="bfed8-282">**Avregistrera provider med prenumerationen**</span><span class="sxs-lookup"><span data-stu-id="bfed8-282">**Unregister provider with the subscription**</span></span>

    provider unregister [options] <namespace>

## <a name="azure-resource-commands-to-manage-your-resources"></a><span data-ttu-id="bfed8-283">Azure-resurs: kommandon för att hantera dina resurser</span><span class="sxs-lookup"><span data-stu-id="bfed8-283">azure resource: Commands to manage your resources</span></span>
<span data-ttu-id="bfed8-284">**Skapar en resurs i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-284">**Creates a resource in a resource group**</span></span>

    resource create [options] <resource-group> <name> <resource-type> <location> <api-version>

<span data-ttu-id="bfed8-285">**Uppdaterar en resurs i en resursgrupp utan parametrar eller mallar**</span><span class="sxs-lookup"><span data-stu-id="bfed8-285">**Updates a resource in a resource group without any templates or parameters**</span></span>

    resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>

<span data-ttu-id="bfed8-286">**Visar en lista över resurser**</span><span class="sxs-lookup"><span data-stu-id="bfed8-286">**Lists the resources**</span></span>

    resource list [options] [resource-group]

<span data-ttu-id="bfed8-287">**Hämtar en resurs i en resursgrupp eller prenumeration**</span><span class="sxs-lookup"><span data-stu-id="bfed8-287">**Gets one resource within a resource group or subscription**</span></span>

    resource show [options] <resource-group> <name> <resource-type> <api-version>

<span data-ttu-id="bfed8-288">**Tar bort en resurs i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-288">**Deletes a resource in a resource group**</span></span>

    resource delete [options] <resource-group> <name> <resource-type> <api-version>

## <a name="azure-role-commands-to-manage-your-azure-roles"></a><span data-ttu-id="bfed8-289">Azure roll: kommandon för att hantera dina Azure roller</span><span class="sxs-lookup"><span data-stu-id="bfed8-289">azure role: Commands to manage your Azure roles</span></span>
<span data-ttu-id="bfed8-290">**Hämta alla tillgängliga rolldefinitioner**</span><span class="sxs-lookup"><span data-stu-id="bfed8-290">**Get all available role definitions**</span></span>

    role list [options]

<span data-ttu-id="bfed8-291">**Hämta en tillgänglig rolldefinitionen**</span><span class="sxs-lookup"><span data-stu-id="bfed8-291">**Get an available role definition**</span></span>

    role show [options] [name]

<span data-ttu-id="bfed8-292">**Kommandon för att hantera din rolltilldelning**</span><span class="sxs-lookup"><span data-stu-id="bfed8-292">**Commands to manage your role assignment**</span></span>

    role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]

## <a name="azure-storage-commands-to-manage-your-storage-objects"></a><span data-ttu-id="bfed8-293">Azure storage: kommandon för att hantera din lagring-objekt</span><span class="sxs-lookup"><span data-stu-id="bfed8-293">azure storage: Commands to manage your Storage objects</span></span>
<span data-ttu-id="bfed8-294">**Kommandon för att hantera dina lagringskonton**</span><span class="sxs-lookup"><span data-stu-id="bfed8-294">**Commands to manage your Storage accounts**</span></span>

    storage account list [options]
    storage account show [options] <name>
    storage account create [options] <name>
    storage account set [options] <name>
    storage account delete [options] <name>

<span data-ttu-id="bfed8-295">**Kommandon för att hantera dina nycklar för Lagringskonto**</span><span class="sxs-lookup"><span data-stu-id="bfed8-295">**Commands to manage your Storage account keys**</span></span>

    storage account keys list [options] <name>
    storage account keys renew [options] <name>

<span data-ttu-id="bfed8-296">**Kommandon för att visa anslutningssträngen för lagring**</span><span class="sxs-lookup"><span data-stu-id="bfed8-296">**Commands to show your Storage connection string**</span></span>

    storage account connectionstring show [options] <name>

<span data-ttu-id="bfed8-297">**Kommandon för att hantera behållare för lagring**</span><span class="sxs-lookup"><span data-stu-id="bfed8-297">**Commands to manage your Storage containers**</span></span>

    storage container list [options] [prefix]
    storage container show [options] [container]
    storage container create [options] [container]
    storage container delete [options] [container]
    storage container set [options] [container]

<span data-ttu-id="bfed8-298">**Kommandon för att hantera delade åtkomst till signaturer för Storage-behållare**</span><span class="sxs-lookup"><span data-stu-id="bfed8-298">**Commands to manage shared access signatures of your Storage container**</span></span>

    storage container sas create [options] [container] [permissions] [expiry]

<span data-ttu-id="bfed8-299">**Kommandon för att hantera lagrade åtkomstprinciper för Storage-behållare**</span><span class="sxs-lookup"><span data-stu-id="bfed8-299">**Commands to manage stored access policies of your Storage container**</span></span>

    storage container policy create [options] [container] [name]
    storage container policy show [options] [container] [name]
    storage container policy list [options] [container]
    storage container policy set [options] [container] [name]
    storage container policy delete [options] [container] [name]

<span data-ttu-id="bfed8-300">**Kommandon för att hantera Storage-blobbar**</span><span class="sxs-lookup"><span data-stu-id="bfed8-300">**Commands to manage your Storage blobs**</span></span>

    storage blob list [options] [container] [prefix]
    storage blob show [options] [container] [blob]
    storage blob delete [options] [container] [blob]
    storage blob upload [options] [file] [container] [blob]
    storage blob download [options] [container] [blob] [destination]

<span data-ttu-id="bfed8-301">**Kommandon för att hantera din blob kopiera åtgärder**</span><span class="sxs-lookup"><span data-stu-id="bfed8-301">**Commands to manage your blob copy operations**</span></span>

    storage blob copy start [options] [sourceUri] [destContainer]
    storage blob copy show [options] [container] [blob]
    storage blob copy stop [options] [container] [blob] [copyid]

<span data-ttu-id="bfed8-302">**Kommandon för att hantera delade åtkomst till signaturen för Storage-blob**</span><span class="sxs-lookup"><span data-stu-id="bfed8-302">**Commands to manage shared access signature of your Storage blob**</span></span>

    storage blob sas create [options] [container] [blob] [permissions] [expiry]

<span data-ttu-id="bfed8-303">**Kommandon för att hantera din lagringsfilresurser**</span><span class="sxs-lookup"><span data-stu-id="bfed8-303">**Commands to manage your Storage file shares**</span></span>

    storage share create [options] [share]
    storage share show [options] [share]
    storage share delete [options] [share]
    storage share list [options] [prefix]

<span data-ttu-id="bfed8-304">**Kommandon för att hantera lagring-filer**</span><span class="sxs-lookup"><span data-stu-id="bfed8-304">**Commands to manage your Storage files**</span></span>

    storage file list [options] [share] [path]
    storage file delete [options] [share] [path]
    storage file upload [options] [source] [share] [path]
    storage file download [options] [share] [path] [destination]

<span data-ttu-id="bfed8-305">**Kommandon för att hantera din lagring filkatalogen**</span><span class="sxs-lookup"><span data-stu-id="bfed8-305">**Commands to manage your Storage file directory**</span></span>

    storage directory create [options] [share] [path]
    storage directory delete [options] [share] [path]

<span data-ttu-id="bfed8-306">**Kommandon för att hantera din lagring-köer**</span><span class="sxs-lookup"><span data-stu-id="bfed8-306">**Commands to manage your Storage queues**</span></span>

    storage queue create [options] [queue]
    storage queue list [options] [prefix]
    storage queue show [options] [queue]
    storage queue delete [options] [queue]

<span data-ttu-id="bfed8-307">**Kommandon för att hantera delade åtkomst till signaturer för Storage-kö**</span><span class="sxs-lookup"><span data-stu-id="bfed8-307">**Commands to manage shared access signatures of your Storage queue**</span></span>

    storage queue sas create [options] [queue] [permissions] [expiry]

<span data-ttu-id="bfed8-308">**Kommandon för att hantera lagrade åtkomstprinciper för Storage-kö**</span><span class="sxs-lookup"><span data-stu-id="bfed8-308">**Commands to manage stored access policies of your Storage queue**</span></span>

    storage queue policy create [options] [queue] [name]
    storage queue policy show [options] [queue] [name]
    storage queue policy list [options] [queue]
    storage queue policy set [options] [queue] [name]
    storage queue policy delete [options] [queue] [name]

<span data-ttu-id="bfed8-309">**Kommandon för att hantera din lagring loggningsegenskaper**</span><span class="sxs-lookup"><span data-stu-id="bfed8-309">**Commands to manage your Storage logging properties**</span></span>

    storage logging show [options]
    storage logging set [options]

<span data-ttu-id="bfed8-310">**Kommandon för att hantera din lagring mått egenskaper**</span><span class="sxs-lookup"><span data-stu-id="bfed8-310">**Commands to manage your Storage metrics properties**</span></span>

    storage metrics show [options]
    storage metrics set [options]

<span data-ttu-id="bfed8-311">**Kommandon för att hantera Storage-tabeller**</span><span class="sxs-lookup"><span data-stu-id="bfed8-311">**Commands to manage your Storage tables**</span></span>

    storage table create [options] [table]
    storage table list [options] [prefix]
    storage table show [options] [table]
    storage table delete [options] [table]

<span data-ttu-id="bfed8-312">**Kommandon för att hantera delade åtkomst till signaturer för din tabell för lagring**</span><span class="sxs-lookup"><span data-stu-id="bfed8-312">**Commands to manage shared access signatures of your Storage table**</span></span>

    storage table sas create [options] [table] [permissions] [expiry]

<span data-ttu-id="bfed8-313">**Kommandon för att hantera lagrade åtkomstprinciper din tabell för lagring**</span><span class="sxs-lookup"><span data-stu-id="bfed8-313">**Commands to manage stored access policies of your Storage table**</span></span>

    storage table policy create [options] [table] [name]
    storage table policy show [options] [table] [name]
    storage table policy list [options] [table]
    storage table policy set [options] [table] [name]
    storage table policy delete [options] [table] [name]

## <a name="azure-tag-commands-to-manage-your-resource-manager-tag"></a><span data-ttu-id="bfed8-314">Azure-taggen: kommandon för att hantera din resource manager-tagg</span><span class="sxs-lookup"><span data-stu-id="bfed8-314">azure tag: Commands to manage your resource manager tag</span></span>
<span data-ttu-id="bfed8-315">**Lägga till en tagg**</span><span class="sxs-lookup"><span data-stu-id="bfed8-315">**Add a tag**</span></span>

    tag create [options] <name> <value>

<span data-ttu-id="bfed8-316">**Ta bort en hel tagg eller ett Taggvärde**</span><span class="sxs-lookup"><span data-stu-id="bfed8-316">**Remove an entire tag or a tag value**</span></span>

    tag delete [options] <name> <value>

<span data-ttu-id="bfed8-317">**Innehåller tagginformation som**</span><span class="sxs-lookup"><span data-stu-id="bfed8-317">**Lists the tag information**</span></span>

    tag list [options]

<span data-ttu-id="bfed8-318">**Hämta en tagg**</span><span class="sxs-lookup"><span data-stu-id="bfed8-318">**Get a tag**</span></span>

    tag show [options] [name]

## <a name="azure-vm-commands-to-manage-your-azure-virtual-machines"></a><span data-ttu-id="bfed8-319">Azure vm: kommandon för att hantera dina Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="bfed8-319">azure vm: Commands to manage your Azure Virtual Machines</span></span>
<span data-ttu-id="bfed8-320">**Skapa en virtuell dator**</span><span class="sxs-lookup"><span data-stu-id="bfed8-320">**Create a VM**</span></span>

    vm create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="bfed8-321">**Skapa en virtuell dator med standardresurser**</span><span class="sxs-lookup"><span data-stu-id="bfed8-321">**Create a VM with default resources**</span></span>

    vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password

> [!TIP]
> <span data-ttu-id="bfed8-322">Från och med CLI version 0.10 kan du ange ett kort alias, till exempel ”UbuntuLTS” eller ”Win2012R2Datacenter” som den `image-urn` för vissa populära Marketplace-bilder.</span><span class="sxs-lookup"><span data-stu-id="bfed8-322">Starting with CLI version 0.10, you can provide a short alias such as "UbuntuLTS" or "Win2012R2Datacenter" as the `image-urn` for some popular Marketplace images.</span></span> <span data-ttu-id="bfed8-323">Kör `azure help vm quick-create` för alternativ.</span><span class="sxs-lookup"><span data-stu-id="bfed8-323">Run `azure help vm quick-create` for options.</span></span> <span data-ttu-id="bfed8-324">Dessutom från och med version 0.10, `azure vm quick-create` använder premium-lagring som standard om den är tillgänglig i den valda regionen.</span><span class="sxs-lookup"><span data-stu-id="bfed8-324">Additionally, starting with version 0.10, `azure vm quick-create` uses premium storage by default if it's available in the selected region.</span></span>
> 
> 

<span data-ttu-id="bfed8-325">**Visa en lista med virtuella datorer i ett konto**</span><span class="sxs-lookup"><span data-stu-id="bfed8-325">**List the virtual machines within an account**</span></span>

    vm list [options]

<span data-ttu-id="bfed8-326">**Hämta en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-326">**Get one virtual machine within a resource group**</span></span>

    vm show [options] <resource-group> <name>

<span data-ttu-id="bfed8-327">**Ta bort en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-327">**Delete one virtual machine within a resource group**</span></span>

    vm delete [options] <resource-group> <name>

<span data-ttu-id="bfed8-328">**Avsluta en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-328">**Shutdown one virtual machine within a resource group**</span></span>

    vm stop [options] <resource-group> <name>

<span data-ttu-id="bfed8-329">**Starta om en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-329">**Restart one virtual machine within a resource group**</span></span>

    vm restart [options] <resource-group> <name>

<span data-ttu-id="bfed8-330">**Starta en virtuell dator i en resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="bfed8-330">**Start one virtual machine within a resource group**</span></span>

    vm start [options] <resource-group> <name>

<span data-ttu-id="bfed8-331">**Avsluta en virtuell dator i en resursgrupp och frigör beräkningsresurser**</span><span class="sxs-lookup"><span data-stu-id="bfed8-331">**Shutdown one virtual machine within a resource group and releases the compute resources**</span></span>

    vm deallocate [options] <resource-group> <name>

<span data-ttu-id="bfed8-332">**Lista över tillgängliga virtuella storlekar**</span><span class="sxs-lookup"><span data-stu-id="bfed8-332">**List available virtual machine sizes**</span></span>

    vm sizes [options]

<span data-ttu-id="bfed8-333">**Spela in VM som OS-avbildningen eller VM-avbildning**</span><span class="sxs-lookup"><span data-stu-id="bfed8-333">**Capture the VM as OS Image or VM Image**</span></span>

    vm capture [options] <resource-group> <name> <vhd-name-prefix>

<span data-ttu-id="bfed8-334">**Ange tillstånd för den virtuella datorn på generaliserad**</span><span class="sxs-lookup"><span data-stu-id="bfed8-334">**Set the state of the VM to Generalized**</span></span>

    vm generalize [options] <resource-group> <name>

<span data-ttu-id="bfed8-335">**Hämta Instansvy för den virtuella datorn**</span><span class="sxs-lookup"><span data-stu-id="bfed8-335">**Get instance view of the VM**</span></span>

    vm get-instance-view [options] <resource-group> <name>

<span data-ttu-id="bfed8-336">**Gör att du vill återställa fjärråtkomst till skrivbordet eller SSH-inställningar på en virtuell dator och för att återställa lösenordet för det konto som har administratör eller sudo-behörighet**</span><span class="sxs-lookup"><span data-stu-id="bfed8-336">**Enable you to reset Remote Desktop Access or SSH settings on a Virtual Machine and to reset the password for the account that has administrator or sudo authority**</span></span>

    vm reset-access [options] <resource-group> <name>

<span data-ttu-id="bfed8-337">**Uppdatera virtuell dator med nya data**</span><span class="sxs-lookup"><span data-stu-id="bfed8-337">**Update VM with new data**</span></span>

    vm set [options] <resource-group> <name>

<span data-ttu-id="bfed8-338">**Kommandon för att hantera dina virtuella hårddiskar**</span><span class="sxs-lookup"><span data-stu-id="bfed8-338">**Commands to manage your Virtual Machine data disks**</span></span>

    vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
    vm disk detach [options] <resource-group> <vm-name> <lun>
    vm disk attach [options] <resource-group> <vm-name> [vhd-url]

<span data-ttu-id="bfed8-339">**Kommandon för att hantera Virtuella resurstillägg**</span><span class="sxs-lookup"><span data-stu-id="bfed8-339">**Commands to manage VM resource extensions**</span></span>

    vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
    vm extension get [options] <resource-group> <vm-name>

<span data-ttu-id="bfed8-340">**Kommandon för att hantera den virtuella datorn Docker**</span><span class="sxs-lookup"><span data-stu-id="bfed8-340">**Commands to manage your Docker Virtual Machine**</span></span>

    vm docker create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="bfed8-341">**Kommandon för att hantera VM-avbildningar**</span><span class="sxs-lookup"><span data-stu-id="bfed8-341">**Commands to manage VM images**</span></span>

    vm image list-publishers [options] <location>
    vm image list-offers [options] <location> <publisher>
    vm image list-skus [options] <location> <publisher> <offer>
    vm image list [options] <location> <publisher> [offer] [sku]
