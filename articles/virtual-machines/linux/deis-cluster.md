---
title: 3-nod Deis aaaDeploy klustret | Microsoft Docs
description: "Den här artikeln beskriver hur toocreate 3-nod Deis kluster på Azure med hjälp av en Azure Resource Manager-mall"
services: virtual-machines-linux
documentationcenter: 
author: HaishiBai
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5eb67eb7-95d4-461d-8eac-44925224ba5f
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/24/2015
ms.author: hbai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a4c0fb8cbb849264e64b433540157c9afecd184e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a><span data-ttu-id="27057-103">Distribuera och konfigurera en 3-nod Deis kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="27057-103">Deploy and configure a 3-node Deis cluster in Azure</span></span>
<span data-ttu-id="27057-104">Den här artikeln vägleder dig genom att etablera en [Deis](http://deis.io/) klustret på Azure.</span><span class="sxs-lookup"><span data-stu-id="27057-104">This article walks you through provisioning a [Deis](http://deis.io/) cluster on Azure.</span></span> <span data-ttu-id="27057-105">Den omfattar alla hello steg från att skapa hello nödvändiga certifikat toodeploying och skala ett exempel på en **Gå** på hello nyetablerade klustret.</span><span class="sxs-lookup"><span data-stu-id="27057-105">It covers all hello steps from creating hello necessary certificates toodeploying and scaling a sample **Go** application on hello newly provisioned cluster.</span></span>

<span data-ttu-id="27057-106">hello visar följande diagram hello arkitekturen för hello distribuerade system.</span><span class="sxs-lookup"><span data-stu-id="27057-106">hello following diagram shows hello architecture of hello deployed system.</span></span> <span data-ttu-id="27057-107">En administratör hanterar hello med Deis verktyg som **deis** och **deisctl**.</span><span class="sxs-lookup"><span data-stu-id="27057-107">A system administrator manages hello cluster using Deis tools such as **deis** and **deisctl**.</span></span> <span data-ttu-id="27057-108">Anslutningar upprättas via en belastningsutjämnare som vidarebefordrar hello anslutningar tooone hello medlem noder i klustret hello Azure.</span><span class="sxs-lookup"><span data-stu-id="27057-108">Connections are established through an Azure load balancer, which forwards hello connections tooone of hello member nodes on hello cluster.</span></span> <span data-ttu-id="27057-109">hello-klienter åtkomst till distribuerade program via hello belastningsutjämnare samt.</span><span class="sxs-lookup"><span data-stu-id="27057-109">hello clients access deployed applications through hello load balancer as well.</span></span> <span data-ttu-id="27057-110">I det här fallet hello belastningsutjämning vidarebefordrar hello trafik tooa Deis router nät som ytterligare routs trafik toocorresponding Docker behållare hello kluster som värd.</span><span class="sxs-lookup"><span data-stu-id="27057-110">In this case, hello load balancer forwards hello traffic tooa Deis router mesh, which further routs traffic toocorresponding Docker containers hosted on hello cluster.</span></span>

  ![Arkitekturdiagram för distribuerade Desis kluster](./media/deis-cluster/architecture-overview.png)

<span data-ttu-id="27057-112">I ordning toorun via följande hello, behöver du:</span><span class="sxs-lookup"><span data-stu-id="27057-112">In order toorun through hello following steps, you'll need:</span></span>

* <span data-ttu-id="27057-113">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="27057-113">An active Azure subscription.</span></span> <span data-ttu-id="27057-114">Om du inte har någon, kan du få en kostnadsfri testversion [azure.com](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="27057-114">If you don't have one, you can get a free trail on [azure.com](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="27057-115">En arbetet eller skolan id toouse Azure-resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="27057-115">A work or school id toouse Azure resource groups.</span></span> <span data-ttu-id="27057-116">Om du har ett personligt konto och logga in med ett Microsoft-id, du behöver för[skapa ett arbetsobjekt-id från din personliga](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27057-116">If you have a personal account and log in with a Microsoft id, you need too[create a work id from your personal one](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="27057-117">Antingen--beroende på dina klientoperativsystem--hello [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller hello [Azure CLI för Mac, Linux och Windows](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="27057-117">Either -- depending on your client operating system -- hello [Azure PowerShell](/powershell/azureps-cmdlets-docs) or hello [Azure CLI for Mac, Linux, and Windows](../../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="27057-118">[OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="27057-118">[OpenSSL](https://www.openssl.org/).</span></span> <span data-ttu-id="27057-119">OpenSSL är används toogenerate hello nödvändiga certifikat.</span><span class="sxs-lookup"><span data-stu-id="27057-119">OpenSSL is used toogenerate hello necessary certificates.</span></span>
* <span data-ttu-id="27057-120">En Git-klient som [Git Bash](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="27057-120">A Git client such as [Git Bash](https://git-scm.com/).</span></span>
* <span data-ttu-id="27057-121">tootest hello exempelprogrammet, måste du också en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="27057-121">tootest hello sample application, you'll also need a DNS server.</span></span> <span data-ttu-id="27057-122">Du kan använda alla DNS-servrar eller tjänster som har stöd för jokertecken A-poster.</span><span class="sxs-lookup"><span data-stu-id="27057-122">You can use any DNS servers or services that support wildcard A records.</span></span>
* <span data-ttu-id="27057-123">En dator toorun Deis klientverktyg.</span><span class="sxs-lookup"><span data-stu-id="27057-123">A computer toorun Deis client tools.</span></span> <span data-ttu-id="27057-124">Du kan använda en lokal dator eller en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="27057-124">You can use either a local machine or a virtual machine.</span></span> <span data-ttu-id="27057-125">Du kan köra dessa verktyg på nästan alla Linux-distribution, men hello följande anvisningar använder Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="27057-125">You can run these tools on almost any Linux distribution, but hello following instructions use Ubuntu.</span></span>

## <a name="provision-hello-cluster"></a><span data-ttu-id="27057-126">Etablera hello kluster</span><span class="sxs-lookup"><span data-stu-id="27057-126">Provision hello cluster</span></span>
<span data-ttu-id="27057-127">I det här avsnittet ska du använda en [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) mall från hello öppen källkod databasen [azure snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="27057-127">In this section, you'll use an [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) template from hello open source repository [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="27057-128">Först ska du kopiera ned hello mallen.</span><span class="sxs-lookup"><span data-stu-id="27057-128">First, you'll copy down hello template.</span></span> <span data-ttu-id="27057-129">Sedan skapar du en ny SSH-nyckel för autentisering.</span><span class="sxs-lookup"><span data-stu-id="27057-129">Then, you'll create a new SSH key pair for authentication.</span></span> <span data-ttu-id="27057-130">Och sedan konfigurerar du en ny identifierare för kluster.</span><span class="sxs-lookup"><span data-stu-id="27057-130">And then, you'll configure a new identifier for you cluster.</span></span> <span data-ttu-id="27057-131">Och slutligen hello Shell-skript eller hello PowerShell-skriptet tooprovision hello klustret ska använda.</span><span class="sxs-lookup"><span data-stu-id="27057-131">And finally, you'll use either hello Shell script or hello PowerShell script tooprovision hello cluster.</span></span>

1. <span data-ttu-id="27057-132">Klona hello databasen: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="27057-132">Clone hello repository: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. <span data-ttu-id="27057-133">Gå toohello mallmapp:</span><span class="sxs-lookup"><span data-stu-id="27057-133">Go toohello template folder:</span></span>
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. <span data-ttu-id="27057-134">Skapa en ny SSH-nyckel med hjälp av ssh-keygen:</span><span class="sxs-lookup"><span data-stu-id="27057-134">Create a new SSH key pair using ssh-keygen:</span></span>
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. <span data-ttu-id="27057-135">Generera ett certifikat med hello ovan privat nyckel:</span><span class="sxs-lookup"><span data-stu-id="27057-135">Generate a certificate using hello above private key:</span></span>
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file toobe generated]
5. <span data-ttu-id="27057-136">Gå för[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate en ny token i klustret som ser ut ungefär som:</span><span class="sxs-lookup"><span data-stu-id="27057-136">Go too[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate a new cluster token, which looks something like:</span></span>
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   <span data-ttu-id="27057-137">Varje virtuell CoreOS-kluster måste toohave en unik token från den här kostnadsfria tjänsten.</span><span class="sxs-lookup"><span data-stu-id="27057-137">Each CoreOS cluster needs toohave a unique token from this free service.</span></span> <span data-ttu-id="27057-138">Se [virtuell CoreOS dokumentationen](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="27057-138">Please see [CoreOS documentation](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) for more details.</span></span>
6. <span data-ttu-id="27057-139">Ändra hello **moln config.yaml** filen tooreplace hello befintliga **identifiering** hello ny token-token:</span><span class="sxs-lookup"><span data-stu-id="27057-139">Modify hello **cloud-config.yaml** file tooreplace hello existing  **discovery** token with hello new token:</span></span>
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment hello following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. <span data-ttu-id="27057-140">Ändra hello **azuredeploy-parameters.json** fil: öppna hello-certifikat som du skapade i steg 4 i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="27057-140">Modify hello **azuredeploy-parameters.json** file: Open hello certificate you created in step 4 in a text editor.</span></span> <span data-ttu-id="27057-141">Kopiera all text mellan `----BEGIN CERTIFICATE-----` och `-----END CERTIFICATE-----` till hello **sshKeyData** parametern (du behöver tooremove alla radmatningstecken).</span><span class="sxs-lookup"><span data-stu-id="27057-141">Copy all text between `----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` into hello **sshKeyData** parameter (you'll need tooremove all newline characters).</span></span>
8. <span data-ttu-id="27057-142">Ändra hello **newStorageAccountName** parameter.</span><span class="sxs-lookup"><span data-stu-id="27057-142">Modify hello **newStorageAccountName** parameter.</span></span> <span data-ttu-id="27057-143">Detta är hello storage-konto för VM OS-diskar.</span><span class="sxs-lookup"><span data-stu-id="27057-143">This is hello storage account for VM OS disks.</span></span> <span data-ttu-id="27057-144">Kontonamnet har toobe globalt unika.</span><span class="sxs-lookup"><span data-stu-id="27057-144">This account name has toobe globally unique.</span></span>
9. <span data-ttu-id="27057-145">Ändra hello **publicDomainName** parameter.</span><span class="sxs-lookup"><span data-stu-id="27057-145">Modify hello **publicDomainName** parameter.</span></span> <span data-ttu-id="27057-146">Detta blir en del av hello DNS-namnet som associeras med hello load balancer offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="27057-146">This will become part of hello DNS name associated with hello load balancer public IP.</span></span> <span data-ttu-id="27057-147">hello slutliga FQDN har hello formatet för *[värdet för den här parametern]*. *[region]* . cloudapp.azure.com. Om du anger hello namn som deishbai32 och hello resursgruppen är distribuerade toohello västra USA region och sedan hello slutliga kommer FQDN tooyour belastningsutjämnaren vara deishbai32.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="27057-147">hello final FQDN will have hello format of *[value of this parameter]*.*[region]*.cloudapp.azure.com. For example, if you specify hello name as deishbai32, and hello resource group is deployed toohello West US region, then hello final FQDN tooyour load balancer will be deishbai32.westus.cloudapp.azure.com.</span></span>
10. <span data-ttu-id="27057-148">Spara hello parameterfil.</span><span class="sxs-lookup"><span data-stu-id="27057-148">Save hello parameter file.</span></span> <span data-ttu-id="27057-149">Och sedan kan du etablera hello-kluster med hjälp av Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="27057-149">And then you can provision hello cluster using Azure PowerShell:</span></span>
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    <span data-ttu-id="27057-150">eller Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="27057-150">or Azure CLI:</span></span>
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. <span data-ttu-id="27057-151">När hello resursgruppen har etablerats visas alla hello resurser i hello grupp på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="27057-151">Once hello resource group is provisioned, you can see all hello resources in hello group on Azure classic portal.</span></span> <span data-ttu-id="27057-152">Som det visas i följande skärmbild, hello hello resursgruppen innehåller ett virtuellt nätverk med tre virtuella datorer, som är anslutna toohello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="27057-152">As shown in hello following screenshot, hello resource group contains a virtual network with three VMs, which are joined toohello same availability set.</span></span> <span data-ttu-id="27057-153">hello gruppen innehåller också en belastningsutjämnare som har en tillhörande offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="27057-153">hello group also contains a load balancer, which has an associated public IP.</span></span>
    
    ![hello etablerade resursgrupp klassiska Azure-portalen](./media/deis-cluster/resource-group.png)

## <a name="install-hello-client"></a><span data-ttu-id="27057-155">Installera hello-klienten</span><span class="sxs-lookup"><span data-stu-id="27057-155">Install hello client</span></span>
<span data-ttu-id="27057-156">Du behöver **deisctl** toocontrol din Deis klustret.</span><span class="sxs-lookup"><span data-stu-id="27057-156">You need **deisctl** toocontrol your Deis cluster.</span></span> <span data-ttu-id="27057-157">Även om deisctl installeras automatiskt på alla noder i klustret hello, är det en bra idé toouse deisctl på en separat administrativ dator.</span><span class="sxs-lookup"><span data-stu-id="27057-157">Although deisctl is automatically installed in all hello cluster nodes, it's a good practice toouse deisctl on a separate administrative machine.</span></span> <span data-ttu-id="27057-158">Dessutom, eftersom alla noder har konfigurerats med endast privata IP-adresser, måste toouse SSH-tunnel via hello belastningsutjämnare, som har en offentlig IP-adress, tooconnect toohello nod datorer.</span><span class="sxs-lookup"><span data-stu-id="27057-158">Furthermore, because all nodes are configured with only private IP addresses, you'll need toouse SSH tunneling through hello load balancer, which has a public IP, tooconnect toohello node machines.</span></span> <span data-ttu-id="27057-159">hello är följande hello stegen för att konfigurera deisctl på en separat Ubuntu fysisk eller virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="27057-159">hello following are hello steps of setting up deisctl on a separate Ubuntu physical or virtual machine.</span></span>

1. <span data-ttu-id="27057-160">Installera deisctl:mkdir deis</span><span class="sxs-lookup"><span data-stu-id="27057-160">Install deisctl:mkdir deis</span></span>
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. <span data-ttu-id="27057-161">Lägg till din privata nyckel toossh agent:</span><span class="sxs-lookup"><span data-stu-id="27057-161">Add your private key toossh agent:</span></span>
   
        eval `ssh-agent -s`
        ssh-add [path toohello private key file, see step 1 in hello previous section]
3. <span data-ttu-id="27057-162">Konfigurera deisctl:</span><span class="sxs-lookup"><span data-stu-id="27057-162">Configure deisctl:</span></span>
   
        export DEISCTL_TUNNEL=[public ip of hello load balancer]:2223

<span data-ttu-id="27057-163">hello mallen definierar ingående NAT-regler som mappar 2223 tooinstance 1, 2224 tooinstance 2 och 2225 tooinstance 3.</span><span class="sxs-lookup"><span data-stu-id="27057-163">hello template defines inbound NAT rules that map 2223 tooinstance 1, 2224 tooinstance 2, and 2225 tooinstance 3.</span></span> <span data-ttu-id="27057-164">Detta ger redundans för hello deisctl verktyget.</span><span class="sxs-lookup"><span data-stu-id="27057-164">This provides redundancy for using hello deisctl tool.</span></span> <span data-ttu-id="27057-165">Du kan granska reglerna på den klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="27057-165">You can examine these rules on Azure classic portal:</span></span>

![NAT-regler för hello belastningsutjämnare](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> <span data-ttu-id="27057-167">Hello mallen stöder för närvarande endast kluster 3-noder.</span><span class="sxs-lookup"><span data-stu-id="27057-167">Currently hello template only supports 3-node clusters.</span></span> <span data-ttu-id="27057-168">Detta är på grund av en begränsning i Azure Resource Manager-mall NAT Regeldefinitionen som inte stöder loop-syntax.</span><span class="sxs-lookup"><span data-stu-id="27057-168">This is because of a limitation in Azure Resource Manager template NAT rule definition, which doesn’t support loop syntax.</span></span>
> 
> 

## <a name="install-and-start-hello-deis-platform"></a><span data-ttu-id="27057-169">Installera och starta hello Deis plattform</span><span class="sxs-lookup"><span data-stu-id="27057-169">Install and start hello Deis platform</span></span>
<span data-ttu-id="27057-170">Nu kan du använda deisctl tooinstall och starta hello Deis plattform:</span><span class="sxs-lookup"><span data-stu-id="27057-170">Now you can use deisctl tooinstall and start hello Deis platform:</span></span>

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path toohello private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> <span data-ttu-id="27057-171">Första hello plattform tar ett tag (upp till 10 minuter).</span><span class="sxs-lookup"><span data-stu-id="27057-171">Starting hello platform takes a while (as much as 10 minutes).</span></span> <span data-ttu-id="27057-172">Särskilt, kan startar hello builder-tjänst ta lång tid.</span><span class="sxs-lookup"><span data-stu-id="27057-172">Especially, starting hello builder service can take a long time.</span></span> <span data-ttu-id="27057-173">Och ibland tar några försöker toosucceed: om hello-åtgärden verkar toohang, försök att skriva `ctrl+c` toobreak körning av hello kommandot och försök igen.</span><span class="sxs-lookup"><span data-stu-id="27057-173">And sometimes it takes a few tries toosucceed: If hello operation seems toohang, try typing `ctrl+c` toobreak execution of hello command and retry.</span></span>
> 
> 

<span data-ttu-id="27057-174">Du kan använda `deisctl list` tooverify om alla tjänster körs:</span><span class="sxs-lookup"><span data-stu-id="27057-174">You can use `deisctl list` tooverify if all services are running:</span></span>

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

<span data-ttu-id="27057-175">Grattis!</span><span class="sxs-lookup"><span data-stu-id="27057-175">Congratulations!</span></span> <span data-ttu-id="27057-176">Nu har du en löpande Deis clsuter i Azure!</span><span class="sxs-lookup"><span data-stu-id="27057-176">Now you've got a running Deis clsuter on Azure!</span></span> <span data-ttu-id="27057-177">Nu ska vi distribuerar ett exempel gå programmet toosee hello klustret i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="27057-177">Next, let's deploy a sample Go application toosee hello cluster in action.</span></span>

## <a name="deploy-and-scale-a-hello-world-application"></a><span data-ttu-id="27057-178">Distribuera och skala en programmet Hello World</span><span class="sxs-lookup"><span data-stu-id="27057-178">Deploy and scale a Hello World application</span></span>
<span data-ttu-id="27057-179">hello följande steg visar hur toodeploy ”Hello World” gå programmet toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="27057-179">hello following steps show how toodeploy a "Hello World" Go application toohello cluster.</span></span> <span data-ttu-id="27057-180">hello steg baseras på [Deis dokumentationen](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span><span class="sxs-lookup"><span data-stu-id="27057-180">hello steps are based on [Deis documentation](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span></span>

1. <span data-ttu-id="27057-181">För hello routning nät toowork korrekt, behöver du toohave en jokertecken A-post för din domän pekar toohello offentliga IP-Adressen för hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="27057-181">For hello routing mesh toowork properly, you’ll need toohave a wildcard A record for your domain pointing toohello public IP of hello load balancer.</span></span> <span data-ttu-id="27057-182">hello följande skärmbild visar hello A-post för en exempel-domänregistrering på GoDaddy:</span><span class="sxs-lookup"><span data-stu-id="27057-182">hello following screenshot shows hello A record for a sample domain registration on GoDaddy:</span></span>
   
    ![GoDaddy A-post](./media/deis-cluster/go-daddy.png)
   
   <p />
2. <span data-ttu-id="27057-184">Installera deis:</span><span class="sxs-lookup"><span data-stu-id="27057-184">Install deis:</span></span>
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. <span data-ttu-id="27057-185">Skapa en ny SSH-nyckel och Lägg sedan till hello offentliga nyckel tooGitHub (naturligtvis kan du också återanvända dina befintliga nycklar).</span><span class="sxs-lookup"><span data-stu-id="27057-185">Create a new SSH key, and then add hello public key tooGitHub (of course, you can also reuse your existing keys).</span></span> <span data-ttu-id="27057-186">toocreate ett nytt SSH nyckelpar, Använd:</span><span class="sxs-lookup"><span data-stu-id="27057-186">toocreate a new SSH key pair, use:</span></span>
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s toouse default file names and empty passcode)
4. <span data-ttu-id="27057-187">Lägga till id_rsa.pub eller hello offentliga nyckeln för ditt val tooGitHub.</span><span class="sxs-lookup"><span data-stu-id="27057-187">Add id_rsa.pub, or hello public key of your choice, tooGitHub.</span></span> <span data-ttu-id="27057-188">Du kan göra detta med hjälp av hello lägga till SSH key-knappen i skärmen SSH-nycklar konfiguration:</span><span class="sxs-lookup"><span data-stu-id="27057-188">You can do this by using hello Add SSH key button in your SSH keys configuration screen:</span></span>
   
   ![GitHub-nyckel](./media/deis-cluster/github-key.png)
   
   <p />
5. <span data-ttu-id="27057-190">Registrera en ny användare:</span><span class="sxs-lookup"><span data-stu-id="27057-190">Register a new user:</span></span>
   
        deis register http://deis.[your domain]
   <p />
6. <span data-ttu-id="27057-191">Lägg till hello SSH-nyckel:</span><span class="sxs-lookup"><span data-stu-id="27057-191">Add hello SSH key:</span></span>
   
        deis keys:add [path tooyour SSH public key]
   <p />      
7. <span data-ttu-id="27057-192">Skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="27057-192">Create an application.</span></span>
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p /><span data-ttu-id="27057-193">
8.Hej git push utlöser Docker bilder toobe skapats och distribuerats, vilket kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="27057-193">
8. hello git push will trigger Docker images toobe built and deployed, which will take a few minutes.</span></span> <span data-ttu-id="27057-194">Från min erfarenhet ibland låser steg 10 (Pushing tooprivate avbildningslagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="27057-194">From my experience, occasionally, Step 10 (Pushing image tooprivate repository) may hang.</span></span> <span data-ttu-id="27057-195">Då kan du stoppa hello process, ta bort hello använder ' deis appar: förstör – a <application name> ` tooremove hello application and try again. You can use `deis apps:list' toofind ut hello namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="27057-195">When this happens, you can stop hello process, remove hello application using `deis apps:destroy –a <application name>` tooremove hello application and try again. You can use `deis apps:list` toofind out hello name of your application.</span></span> <span data-ttu-id="27057-196">Om allt fungerar bör du se något liknande följande hello hello slutet av kommandoutdata:</span><span class="sxs-lookup"><span data-stu-id="27057-196">If everything works out, you should see something like hello following at hello end of command outputs:</span></span>
   
        -----> Launching...
               done, lambda-underdog:v2 deployed tooDeis
               http://lambda-underdog.artitrack.com
               toolearn more, use `deis help` or visit http://deis.io
        toossh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. <span data-ttu-id="27057-197">Kontrollera om hello programmet fungerar:</span><span class="sxs-lookup"><span data-stu-id="27057-197">Verify if hello application is working:</span></span>
   
        curl -S http://[your application name].[your domain]
   <span data-ttu-id="27057-198">Du bör se:</span><span class="sxs-lookup"><span data-stu-id="27057-198">You should see:</span></span>
   
        Welcome tooDeis!
        See hello documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list tooget hello name of your application).
   <p />
10. <span data-ttu-id="27057-199">Skala hello too3 programinstanser:</span><span class="sxs-lookup"><span data-stu-id="27057-199">Scale hello application too3 instances:</span></span>
    
        deis scale cmd=3
    <p />
11. <span data-ttu-id="27057-200">Du kan använda deis info tooexamine information om ditt program.</span><span class="sxs-lookup"><span data-stu-id="27057-200">Optionally, you can use deis info tooexamine details of your application.</span></span> <span data-ttu-id="27057-201">hello är följande utdata från min programdistributionen:</span><span class="sxs-lookup"><span data-stu-id="27057-201">hello following outputs are from my application deployment:</span></span>
    
        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }
    
        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)
    
        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a><span data-ttu-id="27057-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27057-202">Next Steps</span></span>
<span data-ttu-id="27057-203">Den här artikeln gått du igenom alla hello steg tooprovision Deis ett nytt kluster på Azure med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="27057-203">This article walked you through all hello steps tooprovision a new Deis cluster on Azure using an Azure Resource Manager template.</span></span> <span data-ttu-id="27057-204">hello mallen har stöd för redundans i tooling anslutningar samt belastningsutjämning för distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="27057-204">hello template supports redundancy in tooling connections as well as load balancing for deployed applications.</span></span> <span data-ttu-id="27057-205">hello mallen undviker du också använda offentliga IP-adresser för medlem noder, vilket sparar värdefull offentliga IP-resurser och ger en säkrare miljö toohost program.</span><span class="sxs-lookup"><span data-stu-id="27057-205">hello template also avoids using public IPs on member nodes, which saves precious public IP resources and provides a more secured environment toohost applications.</span></span> <span data-ttu-id="27057-206">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="27057-206">toolearn more, see hello following articles:</span></span>

<span data-ttu-id="27057-207">[Översikt över Azure Resource Manager][resource-group-overview]</span><span class="sxs-lookup"><span data-stu-id="27057-207">[Azure Resource Manager Overview][resource-group-overview]</span></span>  
<span data-ttu-id="27057-208">[Hur toouse hello Azure CLI][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="27057-208">[How toouse hello Azure CLI][azure-command-line-tools]</span></span>  
<span data-ttu-id="27057-209">[Använda Azure PowerShell med Azure Resource Manager][powershell-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="27057-209">[Using Azure PowerShell with Azure Resource Manager][powershell-azure-resource-manager]</span></span>  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
