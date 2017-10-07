---
title: aaaUse tom edge-noder i Hadoop-kluster i HDInsight - Azure | Microsoft Docs
description: "Hur tooadd en tom edge nod tooan HDInsight-kluster som kan användas som en klient och sedan testa/host HDInsight-program."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: 9c910905b51f2fe92e6e5d47d86a32bd5247c2cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="3db0a-103">Använda tom edge noder på Hadoop-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3db0a-103">Use empty edge nodes on Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="3db0a-104">Lär dig hur tooadd en tom kant nod tooan HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-104">Learn how tooadd an empty edge node tooan HDInsight cluster.</span></span> <span data-ttu-id="3db0a-105">En tom kantnod är en virtuell Linux-dator med hello samma klientverktyg installeras och konfigureras som hello headnodes, men utan några Hadoop-tjänster som körs.</span><span class="sxs-lookup"><span data-stu-id="3db0a-105">An empty edge node is a Linux virtual machine with hello same client tools installed and configured as in hello headnodes, but with no Hadoop services running.</span></span> <span data-ttu-id="3db0a-106">Du kan använda hello kantnod för att komma åt hello kluster, testa ditt klientprogram och värd för ditt program.</span><span class="sxs-lookup"><span data-stu-id="3db0a-106">You can use hello edge node for accessing hello cluster, testing your client applications, and hosting your client applications.</span></span> 

<span data-ttu-id="3db0a-107">Du kan lägga till ett tomt edge-noden tooan befintligt HDInsight-kluster, tooa nytt kluster när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-107">You can add an empty edge node tooan existing HDInsight cluster, tooa new cluster when you create hello cluster.</span></span> <span data-ttu-id="3db0a-108">Lägga till en tom kantnod görs med hjälp av Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="3db0a-108">Adding an empty edge node is done using Azure Resource Manager template.</span></span>  <span data-ttu-id="3db0a-109">hello följande exempel visar hur du gör med hjälp av en mall:</span><span class="sxs-lookup"><span data-stu-id="3db0a-109">hello following sample demonstrates how it is done using a template:</span></span>

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

<span data-ttu-id="3db0a-110">Som det visas i exemplet hello, kan du om du vill anropa en [skript åtgärd](hdinsight-hadoop-customize-cluster-linux.md) tooperform ytterligare konfigurering, till exempel installera [Apache Hue](hdinsight-hadoop-hue-linux.md) i hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-110">As shown in hello sample, you can optionally call a [script action](hdinsight-hadoop-customize-cluster-linux.md) tooperform additional configuration, such as installing [Apache Hue](hdinsight-hadoop-hue-linux.md) in hello edge node.</span></span> <span data-ttu-id="3db0a-111">hello skriptet åtgärdsskriptet måste vara allmänt tillgänglig på hello-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="3db0a-111">hello script action script must be publicly accessible on hello web.</span></span>  <span data-ttu-id="3db0a-112">Till exempel om hello skriptet lagras i Azure storage, använda offentliga behållare eller offentliga blobar.</span><span class="sxs-lookup"><span data-stu-id="3db0a-112">For example, if hello script is stored in Azure storage, use either public containers or public blobs.</span></span>

<span data-ttu-id="3db0a-113">hello edge nodstorlek virtuella måste uppfylla hello HDInsight-kluster worker nod vm storlek krav.</span><span class="sxs-lookup"><span data-stu-id="3db0a-113">hello edge node virtual machine size must meet hello HDInsight cluster worker node vm size requirements.</span></span> <span data-ttu-id="3db0a-114">Hello rekommenderade worker nod vm-storlekar, finns [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="3db0a-114">For hello recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>

<span data-ttu-id="3db0a-115">När du har skapat en kantnod kan du ansluta toohello kantnod med SSH och kör klienten verktyg tooaccess hello Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3db0a-115">After you have created an edge node, you can connect toohello edge node using SSH, and run client tools tooaccess hello Hadoop cluster in HDInsight.</span></span>

> [!WARNING] 
> <span data-ttu-id="3db0a-116">Använda en tom kantnod med HDInsight är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="3db0a-116">Using an empty edge node with HDInsight is currently in preview.</span></span> <span data-ttu-id="3db0a-117">Anpassade komponenter som är installerade på hello kantnod få kommersiellt rimliga support från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3db0a-117">Custom components that are installed on hello edge node receive commercially reasonable support from Microsoft.</span></span> <span data-ttu-id="3db0a-118">Detta kan resultera i att lösa eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="3db0a-118">This might result in resolving problems you encounter.</span></span> <span data-ttu-id="3db0a-119">Eller du hänvisade toocommunity resurser för ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="3db0a-119">Or you may be referred toocommunity resources for further assistance.</span></span> <span data-ttu-id="3db0a-120">hello följande är några av de flesta aktiva platser för att få hjälp från hello community hello:</span><span class="sxs-lookup"><span data-stu-id="3db0a-120">hello following are some of hello most active sites for getting help from hello community:</span></span>
>
> * [<span data-ttu-id="3db0a-121">MSDN-forum för HDInsight</span><span class="sxs-lookup"><span data-stu-id="3db0a-121">MSDN forum for HDInsight</span></span>](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * <span data-ttu-id="3db0a-122">[http://StackOverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="3db0a-122">[http://stackoverflow.com](http://stackoverflow.com).</span></span>
>
> <span data-ttu-id="3db0a-123">Om du använder en Apache-teknik kan du kanske kan toofind hjälp via hello Apache projektwebbplatser på [http://apache.org](http://apache.org), till exempel hello [Hadoop](http://hadoop.apache.org/) plats.</span><span class="sxs-lookup"><span data-stu-id="3db0a-123">If you are using an Apache technology, you may be able toofind assistance through hello Apache project sites on [http://apache.org](http://apache.org), such as hello [Hadoop](http://hadoop.apache.org/) site.</span></span>

## <a name="add-an-edge-node-tooan-existing-cluster"></a><span data-ttu-id="3db0a-124">Lägg till ett befintligt kluster för edge noden tooan</span><span class="sxs-lookup"><span data-stu-id="3db0a-124">Add an edge node tooan existing cluster</span></span>
<span data-ttu-id="3db0a-125">I det här avsnittet använder du en Resource Manager mallen tooadd ett edge nod tooan befintliga HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-125">In this section, you use a Resource Manager template tooadd an edge node tooan existing HDInsight cluster.</span></span>  <span data-ttu-id="3db0a-126">hello Resource Manager-mall finns i [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="3db0a-126">hello Resource Manager template can be found in [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/).</span></span> <span data-ttu-id="3db0a-127">hello Resource Manager-mall anropar en skriptåtgärd på https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. hello skript utföra inte några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3db0a-127">hello Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. hello script doesn't perform any actions.</span></span>  <span data-ttu-id="3db0a-128">Det är toodemonstrate anropar skriptåtgärd från en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="3db0a-128">It is toodemonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="3db0a-129">**tooadd ett tomt edge nod tooan befintligt kluster**</span><span class="sxs-lookup"><span data-stu-id="3db0a-129">**tooadd an empty edge node tooan existing cluster**</span></span>

1. <span data-ttu-id="3db0a-130">Skapa ett HDInsight-kluster om du inte har någon ännu.</span><span class="sxs-lookup"><span data-stu-id="3db0a-130">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="3db0a-131">Se [Hadoop-vägledning: komma igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3db0a-131">See [Hadoop tutorial: Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="3db0a-132">Klicka på hello efter bilden toosign i tooAzure och öppna hello Azure Resource Manager-mall i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3db0a-132">Click hello following image toosign in tooAzure and open hello Azure Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. <span data-ttu-id="3db0a-133">Konfigurera hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3db0a-133">Configure hello following properties:</span></span>
   
   * <span data-ttu-id="3db0a-134">**Prenumerationen**: Välj en Azure-prenumeration används för att skapa hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3db0a-134">**Subscription**: Select an Azure subscription used for creating hello cluster.</span></span>
   * <span data-ttu-id="3db0a-135">**Resursgruppen**: Välj hello resursgruppens namn används för hello befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-135">**Resource group**: Select hello resource group used for hello existing HDInsight cluster.</span></span>
   * <span data-ttu-id="3db0a-136">**Plats**: välja hello platsen för hello befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-136">**Location**: Select hello location of hello existing HDInsight cluster.</span></span>
   * <span data-ttu-id="3db0a-137">**Klusternamn**: Ange hello namnet på ett befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-137">**Cluster Name**: Enter hello name of an existing HDInsight cluster.</span></span>
   * <span data-ttu-id="3db0a-138">**Kant nodstorlek**: Välj något av hello VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="3db0a-138">**Edge Node Size**: Select one of hello VM sizes.</span></span> <span data-ttu-id="3db0a-139">hello vm-storlek måste uppfylla hello worker nod vm storlek krav.</span><span class="sxs-lookup"><span data-stu-id="3db0a-139">hello vm size must meet hello worker node vm size requirements.</span></span> <span data-ttu-id="3db0a-140">Hello rekommenderade worker nod vm-storlekar, finns [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span><span class="sxs-lookup"><span data-stu-id="3db0a-140">For hello recommended worker node vm sizes, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).</span></span>
   * <span data-ttu-id="3db0a-141">**Kant nod prefixet**: hello standardvärdet är **nya**.</span><span class="sxs-lookup"><span data-stu-id="3db0a-141">**Edge Node Prefix**: hello default value is **new**.</span></span>  <span data-ttu-id="3db0a-142">Med standardvärdet för hello hello edge nodnamnet är **nya edgenode**.</span><span class="sxs-lookup"><span data-stu-id="3db0a-142">Using hello default value, hello edge node name is **new-edgenode**.</span></span>  <span data-ttu-id="3db0a-143">Du kan anpassa hello prefix från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3db0a-143">You can customize hello prefix from hello portal.</span></span> <span data-ttu-id="3db0a-144">Du kan också anpassa hello fullständiga namn från hello mall.</span><span class="sxs-lookup"><span data-stu-id="3db0a-144">You can also customize hello full name from hello template.</span></span>

4. <span data-ttu-id="3db0a-145">Kontrollera **acceptera toohello villkoren ovan**, och klicka sedan på **inköp** toocreate hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-145">Check **I agree toohello terms and conditions stated above**, and then click  **Purchase** toocreate hello edge node.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="3db0a-146">Se till att tooselect hello Azure-resursgrupp för hello befintliga HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-146">Make sure tooselect hello Azure resource group for hello existing HDInsight cluster.</span></span>  <span data-ttu-id="3db0a-147">Annars felmeddelande hello meddelandet ”kan inte utföra åtgärden på kapslad resurs.</span><span class="sxs-lookup"><span data-stu-id="3db0a-147">Otherwise, you get hello error message "Can not perform requested operation on nested resource.</span></span> <span data-ttu-id="3db0a-148">Överordnade resursen '&lt;klusternamn >' hittades inte ”.</span><span class="sxs-lookup"><span data-stu-id="3db0a-148">Parent resource '&lt;ClusterName>' not found."</span></span>

## <a name="add-an-edge-node-when-creating-a-cluster"></a><span data-ttu-id="3db0a-149">Lägg till en kantnod när du skapar ett kluster</span><span class="sxs-lookup"><span data-stu-id="3db0a-149">Add an edge node when creating a cluster</span></span>
<span data-ttu-id="3db0a-150">I det här avsnittet använder du en Resource Manager mallen toocreate HDInsight-kluster med en kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-150">In this section, you use a Resource Manager template toocreate HDInsight cluster with an edge node.</span></span>  <span data-ttu-id="3db0a-151">hello Resource Manager-mall finns i hello [Azure QuickStart mallgalleriet](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span><span class="sxs-lookup"><span data-stu-id="3db0a-151">hello Resource Manager template can be found in hello [Azure QuickStart Templates gallery](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/).</span></span> <span data-ttu-id="3db0a-152">hello Resource Manager-mall anropar en skriptåtgärd på https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. hello skript utföra inte några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3db0a-152">hello Resource Manager template calls a script action located at https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. hello script doesn't perform any actions.</span></span>  <span data-ttu-id="3db0a-153">Det är toodemonstrate anropar skriptåtgärd från en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="3db0a-153">It is toodemonstrate calling script action from a Resource Manager template.</span></span>

<span data-ttu-id="3db0a-154">**tooadd ett tomt edge nod tooan befintligt kluster**</span><span class="sxs-lookup"><span data-stu-id="3db0a-154">**tooadd an empty edge node tooan existing cluster**</span></span>

1. <span data-ttu-id="3db0a-155">Skapa ett HDInsight-kluster om du inte har någon ännu.</span><span class="sxs-lookup"><span data-stu-id="3db0a-155">Create an HDInsight cluster if you don't have one yet.</span></span>  <span data-ttu-id="3db0a-156">Se [komma igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3db0a-156">See [Get started using Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
2. <span data-ttu-id="3db0a-157">Klicka på hello efter bilden toosign i tooAzure och öppna hello Azure Resource Manager-mall i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3db0a-157">Click hello following image toosign in tooAzure and open hello Azure Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. <span data-ttu-id="3db0a-158">Konfigurera hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3db0a-158">Configure hello following properties:</span></span>
   
   * <span data-ttu-id="3db0a-159">**Prenumerationen**: Välj en Azure-prenumeration används för att skapa hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3db0a-159">**Subscription**: Select an Azure subscription used for creating hello cluster.</span></span>
   * <span data-ttu-id="3db0a-160">**Resursgruppen**: skapa en ny resursgrupp som används för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3db0a-160">**Resource group**: Create a new resource group used for hello cluster.</span></span>
   * <span data-ttu-id="3db0a-161">**Plats**: Välj en plats för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3db0a-161">**Location**: Select a location for hello resource group.</span></span>
   * <span data-ttu-id="3db0a-162">**Klusternamn**: Ange ett namn för hello nya kluster toocreate.</span><span class="sxs-lookup"><span data-stu-id="3db0a-162">**Cluster Name**: Enter a name for hello new cluster toocreate.</span></span>
   * <span data-ttu-id="3db0a-163">**Klustrets inloggningsnamn**: Ange hello Hadoop HTTP-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3db0a-163">**Cluster Login User Name**: Enter hello Hadoop HTTP user name.</span></span>  <span data-ttu-id="3db0a-164">hello standardnamnet är **admin**.</span><span class="sxs-lookup"><span data-stu-id="3db0a-164">hello default name is **admin**.</span></span>
   * <span data-ttu-id="3db0a-165">**Klustret inloggningslösenordet**: Ange hello Hadoop HTTP användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="3db0a-165">**Cluster Login Password**: Enter hello Hadoop HTTP user password.</span></span>
   * <span data-ttu-id="3db0a-166">**SSH användarnamn**: Ange hello SSH-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3db0a-166">**Ssh User Name**: Enter hello SSH user name.</span></span> <span data-ttu-id="3db0a-167">hello standardnamnet är **sshuser**.</span><span class="sxs-lookup"><span data-stu-id="3db0a-167">hello default name is **sshuser**.</span></span>
   * <span data-ttu-id="3db0a-168">**SSH lösenord**: Ange hello SSH användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="3db0a-168">**Ssh Password**: Enter hello SSH user password.</span></span>
   * <span data-ttu-id="3db0a-169">**Installera skriptåtgärd**: Behåll standardvärdet för hello för att gå igenom den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3db0a-169">**Install Script Action**: Keep hello default value for going through this tutorial.</span></span>
     
     <span data-ttu-id="3db0a-170">Vissa egenskaper har hårdkodad i hello mall: typ av kluster, kluster worker-nodsantalet, Edge nodstorlek och Edge nodnamn.</span><span class="sxs-lookup"><span data-stu-id="3db0a-170">Some properties have been hardcoded in hello template: Cluster type, Cluster worker node count, Edge node size, and Edge node name.</span></span>
4. <span data-ttu-id="3db0a-171">Kontrollera **acceptera toohello villkoren ovan**, och klicka sedan på **inköp** toocreate hello kluster med hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-171">Check **I agree toohello terms and conditions stated above**, and then click  **Purchase** toocreate hello cluster with hello edge node.</span></span>

## <a name="access-an-edge-node"></a><span data-ttu-id="3db0a-172">Komma åt en kantnod</span><span class="sxs-lookup"><span data-stu-id="3db0a-172">Access an edge node</span></span>
<span data-ttu-id="3db0a-173">hello kantnod ssh-slutpunkten är &lt;EdgeNodeName >.&lt; Klusternamn >-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="3db0a-173">hello edge node ssh endpoint is &lt;EdgeNodeName>.&lt;ClusterName>-ssh.azurehdinsight.net:22.</span></span>  <span data-ttu-id="3db0a-174">Till exempel nya-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span><span class="sxs-lookup"><span data-stu-id="3db0a-174">For example, new-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.</span></span>

<span data-ttu-id="3db0a-175">hello kantnod visas som ett program på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3db0a-175">hello edge node appears as an application on hello Azure portal.</span></span>  <span data-ttu-id="3db0a-176">hello portal ger du hello information tooaccess hello kant noden med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="3db0a-176">hello portal gives you hello information tooaccess hello edge node using SSH.</span></span>

<span data-ttu-id="3db0a-177">**tooverify hello edge nod SSH slutpunkt**</span><span class="sxs-lookup"><span data-stu-id="3db0a-177">**tooverify hello edge node SSH endpoint**</span></span>

1. <span data-ttu-id="3db0a-178">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3db0a-178">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3db0a-179">Öppna hello HDInsight-kluster med en kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-179">Open hello HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="3db0a-180">Klicka på **program** från hello klusterbladet.</span><span class="sxs-lookup"><span data-stu-id="3db0a-180">Click **Applications** from hello cluster blade.</span></span> <span data-ttu-id="3db0a-181">Du bör se hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-181">You shall see hello edge node.</span></span>  <span data-ttu-id="3db0a-182">hello standardnamnet är **nya edgenode**.</span><span class="sxs-lookup"><span data-stu-id="3db0a-182">hello default name is **new-edgenode**.</span></span>
4. <span data-ttu-id="3db0a-183">Klicka på hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-183">Click hello edge node.</span></span> <span data-ttu-id="3db0a-184">Du bör se hello SSH-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="3db0a-184">You shall see hello SSH endpoint.</span></span>

<span data-ttu-id="3db0a-185">**toouse Hive på hello kantnod**</span><span class="sxs-lookup"><span data-stu-id="3db0a-185">**toouse Hive on hello edge node**</span></span>

1. <span data-ttu-id="3db0a-186">Använda SSH tooconnect toohello kantnoden.</span><span class="sxs-lookup"><span data-stu-id="3db0a-186">Use SSH tooconnect toohello edge node.</span></span> <span data-ttu-id="3db0a-187">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3db0a-187">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="3db0a-188">När du har anslutit toohello kantnod med SSH, Använd följande Hive kommandokonsolen tooopen hello hello:</span><span class="sxs-lookup"><span data-stu-id="3db0a-188">After you have connected toohello edge node using SSH, use hello following command tooopen hello Hive console:</span></span>
   
        hive
3. <span data-ttu-id="3db0a-189">Hello kör följande kommando tooshow Hive-tabeller i hello kluster:</span><span class="sxs-lookup"><span data-stu-id="3db0a-189">Run hello following command tooshow Hive tables in hello cluster:</span></span>
   
        show tables;

## <a name="delete-an-edge-node"></a><span data-ttu-id="3db0a-190">Ta bort en kantnod</span><span class="sxs-lookup"><span data-stu-id="3db0a-190">Delete an edge node</span></span>
<span data-ttu-id="3db0a-191">Du kan ta bort en kantnod hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3db0a-191">You can delete an edge node from hello Azure portal.</span></span>

<span data-ttu-id="3db0a-192">**tooaccess en kantnod**</span><span class="sxs-lookup"><span data-stu-id="3db0a-192">**tooaccess an edge node**</span></span>

1. <span data-ttu-id="3db0a-193">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3db0a-193">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3db0a-194">Öppna hello HDInsight-kluster med en kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-194">Open hello HDInsight cluster with an edge node.</span></span>
3. <span data-ttu-id="3db0a-195">Klicka på **program** från hello klusterbladet.</span><span class="sxs-lookup"><span data-stu-id="3db0a-195">Click **Applications** from hello cluster blade.</span></span> <span data-ttu-id="3db0a-196">Visas en lista över edge noder.</span><span class="sxs-lookup"><span data-stu-id="3db0a-196">You shall see a list of edge nodes.</span></span>  
4. <span data-ttu-id="3db0a-197">Högerklicka på hello kantnod du toodelete, och klickar sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="3db0a-197">Right-click hello edge node you want toodelete, and then click **Delete**.</span></span>
5. <span data-ttu-id="3db0a-198">Klicka på **Ja** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="3db0a-198">Click **Yes** tooconfirm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3db0a-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3db0a-199">Next steps</span></span>
<span data-ttu-id="3db0a-200">I den här artikeln har du lärt dig hur tooadd en kantnod och hur tooaccess hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="3db0a-200">In this article, you have learned how tooadd an edge node and how tooaccess hello edge node.</span></span> <span data-ttu-id="3db0a-201">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="3db0a-201">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="3db0a-202">[Installera HDInsight-program](hdinsight-apps-install-applications.md): Lär dig hur tooinstall tooyour för programmet ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-202">[Install HDInsight applications](hdinsight-apps-install-applications.md): Learn how tooinstall an HDInsight application tooyour clusters.</span></span>
* <span data-ttu-id="3db0a-203">[Installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md): Lär dig hur toodeploy tooHDInsight för ett Opublicerat HDInsight-program.</span><span class="sxs-lookup"><span data-stu-id="3db0a-203">[Install custom HDInsight applications](hdinsight-apps-install-custom-applications.md): learn how toodeploy an unpublished HDInsight application tooHDInsight.</span></span>
* <span data-ttu-id="3db0a-204">[Publicera HDInsight-program](hdinsight-apps-publish-applications.md): Lär dig hur toopublish din anpassade HDInsight-program tooAzure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="3db0a-204">[Publish HDInsight applications](hdinsight-apps-publish-applications.md): Learn how toopublish your custom HDInsight applications tooAzure Marketplace.</span></span>
* <span data-ttu-id="3db0a-205">[MSDN: Installera ett HDInsight-program](https://msdn.microsoft.com/library/mt706515.aspx): Lär dig hur toodefine HDInsight-program.</span><span class="sxs-lookup"><span data-stu-id="3db0a-205">[MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx): Learn how toodefine HDInsight applications.</span></span>
* <span data-ttu-id="3db0a-206">[Anpassa Linux-baserat HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md): Lär dig hur toouse skriptåtgärd tooinstall fler program.</span><span class="sxs-lookup"><span data-stu-id="3db0a-206">[Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md): learn how toouse Script Action tooinstall additional applications.</span></span>
* <span data-ttu-id="3db0a-207">[Skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Lär dig hur toocall Resource Manager-mallar toocreate HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db0a-207">[Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md): learn how toocall Resource Manager templates toocreate HDInsight clusters.</span></span>

