---
title: Distribuera en 3-nod Deis klustret | Microsoft Docs
description: "Den här artikeln beskriver hur du skapar en 3-nod Deis kluster på Azure med hjälp av en Azure Resource Manager-mall"
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
ms.openlocfilehash: 9a0c3dd7562dfb5ce54c2ebfd4665109f59cd8fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a><span data-ttu-id="5c781-103">Distribuera och konfigurera en 3-nod Deis kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="5c781-103">Deploy and configure a 3-node Deis cluster in Azure</span></span>
<span data-ttu-id="5c781-104">Den här artikeln vägleder dig genom att etablera en [Deis](http://deis.io/) klustret på Azure.</span><span class="sxs-lookup"><span data-stu-id="5c781-104">This article walks you through provisioning a [Deis](http://deis.io/) cluster on Azure.</span></span> <span data-ttu-id="5c781-105">Den omfattar alla steg från att skapa nödvändiga certifikat för att distribuera och skala ett exempel på en **Gå** på nyetablerade klustret.</span><span class="sxs-lookup"><span data-stu-id="5c781-105">It covers all the steps from creating the necessary certificates to deploying and scaling a sample **Go** application on the newly provisioned cluster.</span></span>

<span data-ttu-id="5c781-106">Följande diagram visar arkitekturen för det distribuerade systemet.</span><span class="sxs-lookup"><span data-stu-id="5c781-106">The following diagram shows the architecture of the deployed system.</span></span> <span data-ttu-id="5c781-107">En administratör hanterar klustret med hjälp av Deis verktyg som **deis** och **deisctl**.</span><span class="sxs-lookup"><span data-stu-id="5c781-107">A system administrator manages the cluster using Deis tools such as **deis** and **deisctl**.</span></span> <span data-ttu-id="5c781-108">Anslutningar upprättas via en Azure belastningsutjämnare som vidarebefordrar anslutningar till en medlemmen noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="5c781-108">Connections are established through an Azure load balancer, which forwards the connections to one of the member nodes on the cluster.</span></span> <span data-ttu-id="5c781-109">Klienterna kommer åt distribuerade program via belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="5c781-109">The clients access deployed applications through the load balancer as well.</span></span> <span data-ttu-id="5c781-110">I det här fallet belastningsutjämnaren vidarebefordrar trafik till en Deis router nät som ytterligare routs trafik till motsvarande Docker-behållare i värdklustret.</span><span class="sxs-lookup"><span data-stu-id="5c781-110">In this case, the load balancer forwards the traffic to a Deis router mesh, which further routs traffic to corresponding Docker containers hosted on the cluster.</span></span>

  ![Arkitekturdiagram för distribuerade Desis kluster](./media/deis-cluster/architecture-overview.png)

<span data-ttu-id="5c781-112">För att kunna köra igenom följande steg behöver du:</span><span class="sxs-lookup"><span data-stu-id="5c781-112">In order to run through the following steps, you'll need:</span></span>

* <span data-ttu-id="5c781-113">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5c781-113">An active Azure subscription.</span></span> <span data-ttu-id="5c781-114">Om du inte har någon, kan du få en kostnadsfri testversion [azure.com](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="5c781-114">If you don't have one, you can get a free trail on [azure.com](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="5c781-115">Ett arbets- eller skolkonto id för att använda Azure-resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="5c781-115">A work or school id to use Azure resource groups.</span></span> <span data-ttu-id="5c781-116">Om du har ett personligt konto och logga in med ett Microsoft-id kan du behöva [skapa ett arbetsobjekt-id från din personliga](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c781-116">If you have a personal account and log in with a Microsoft id, you need to [create a work id from your personal one](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="5c781-117">Antingen--beroende på dina klientoperativsystem--den [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller [Azure CLI för Mac, Linux och Windows](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5c781-117">Either -- depending on your client operating system -- the [Azure PowerShell](/powershell/azureps-cmdlets-docs) or the [Azure CLI for Mac, Linux, and Windows](../../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="5c781-118">[OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="5c781-118">[OpenSSL](https://www.openssl.org/).</span></span> <span data-ttu-id="5c781-119">OpenSSL används för att generera nödvändiga certifikat.</span><span class="sxs-lookup"><span data-stu-id="5c781-119">OpenSSL is used to generate the necessary certificates.</span></span>
* <span data-ttu-id="5c781-120">En Git-klient som [Git Bash](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="5c781-120">A Git client such as [Git Bash](https://git-scm.com/).</span></span>
* <span data-ttu-id="5c781-121">Om du vill testa exempelprogrammet måste du också en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="5c781-121">To test the sample application, you'll also need a DNS server.</span></span> <span data-ttu-id="5c781-122">Du kan använda alla DNS-servrar eller tjänster som har stöd för jokertecken A-poster.</span><span class="sxs-lookup"><span data-stu-id="5c781-122">You can use any DNS servers or services that support wildcard A records.</span></span>
* <span data-ttu-id="5c781-123">En dator för att köra Deis klientverktyg.</span><span class="sxs-lookup"><span data-stu-id="5c781-123">A computer to run Deis client tools.</span></span> <span data-ttu-id="5c781-124">Du kan använda en lokal dator eller en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5c781-124">You can use either a local machine or a virtual machine.</span></span> <span data-ttu-id="5c781-125">Du kan köra dessa verktyg på nästan alla Linux-distribution, men följande instruktioner använda Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="5c781-125">You can run these tools on almost any Linux distribution, but the following instructions use Ubuntu.</span></span>

## <a name="provision-the-cluster"></a><span data-ttu-id="5c781-126">Etablera klustret</span><span class="sxs-lookup"><span data-stu-id="5c781-126">Provision the cluster</span></span>
<span data-ttu-id="5c781-127">I det här avsnittet ska du använda en [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) mallen från lagringsplatsen för öppen källkod [azure snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="5c781-127">In this section, you'll use an [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) template from the open source repository [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="5c781-128">Först ska du kopiera ned mallen.</span><span class="sxs-lookup"><span data-stu-id="5c781-128">First, you'll copy down the template.</span></span> <span data-ttu-id="5c781-129">Sedan skapar du en ny SSH-nyckel för autentisering.</span><span class="sxs-lookup"><span data-stu-id="5c781-129">Then, you'll create a new SSH key pair for authentication.</span></span> <span data-ttu-id="5c781-130">Och sedan konfigurerar du en ny identifierare för kluster.</span><span class="sxs-lookup"><span data-stu-id="5c781-130">And then, you'll configure a new identifier for you cluster.</span></span> <span data-ttu-id="5c781-131">Och slutligen du ska använda Shell-skript eller PowerShell-skript för att etablera klustret.</span><span class="sxs-lookup"><span data-stu-id="5c781-131">And finally, you'll use either the Shell script or the PowerShell script to provision the cluster.</span></span>

1. <span data-ttu-id="5c781-132">Klona lagringsplatsen: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="5c781-132">Clone the repository: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. <span data-ttu-id="5c781-133">Gå till mallmappen:</span><span class="sxs-lookup"><span data-stu-id="5c781-133">Go to the template folder:</span></span>
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. <span data-ttu-id="5c781-134">Skapa en ny SSH-nyckel med hjälp av ssh-keygen:</span><span class="sxs-lookup"><span data-stu-id="5c781-134">Create a new SSH key pair using ssh-keygen:</span></span>
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. <span data-ttu-id="5c781-135">Generera ett certifikat med den privata nyckeln ovan:</span><span class="sxs-lookup"><span data-stu-id="5c781-135">Generate a certificate using the above private key:</span></span>
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]
5. <span data-ttu-id="5c781-136">Gå till [https://discovery.etcd.io/new](https://discovery.etcd.io/new) att skapa en ny token i klustret, som ser ut ungefär som:</span><span class="sxs-lookup"><span data-stu-id="5c781-136">Go to [https://discovery.etcd.io/new](https://discovery.etcd.io/new) to generate a new cluster token, which looks something like:</span></span>
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   <span data-ttu-id="5c781-137">Varje virtuell CoreOS-kluster måste ha en unik token från den här kostnadsfria tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5c781-137">Each CoreOS cluster needs to have a unique token from this free service.</span></span> <span data-ttu-id="5c781-138">Se [virtuell CoreOS dokumentationen](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="5c781-138">Please see [CoreOS documentation](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) for more details.</span></span>
6. <span data-ttu-id="5c781-139">Ändra den **moln config.yaml** ersätta den befintliga filen **identifiering** token med den nya token:</span><span class="sxs-lookup"><span data-stu-id="5c781-139">Modify the **cloud-config.yaml** file to replace the existing  **discovery** token with the new token:</span></span>
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. <span data-ttu-id="5c781-140">Ändra den **azuredeploy-parameters.json** fil: öppna certifikatet som du skapade i steg 4 i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="5c781-140">Modify the **azuredeploy-parameters.json** file: Open the certificate you created in step 4 in a text editor.</span></span> <span data-ttu-id="5c781-141">Kopiera all text mellan `----BEGIN CERTIFICATE-----` och `-----END CERTIFICATE-----` till den **sshKeyData** parametern (du behöver ta bort alla radmatningstecken).</span><span class="sxs-lookup"><span data-stu-id="5c781-141">Copy all text between `----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` into the **sshKeyData** parameter (you'll need to remove all newline characters).</span></span>
8. <span data-ttu-id="5c781-142">Ändra den **newStorageAccountName** parameter.</span><span class="sxs-lookup"><span data-stu-id="5c781-142">Modify the **newStorageAccountName** parameter.</span></span> <span data-ttu-id="5c781-143">Det här är lagringskontot för VM OS-diskar.</span><span class="sxs-lookup"><span data-stu-id="5c781-143">This is the storage account for VM OS disks.</span></span> <span data-ttu-id="5c781-144">Kontonamnet måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="5c781-144">This account name has to be globally unique.</span></span>
9. <span data-ttu-id="5c781-145">Ändra den **publicDomainName** parameter.</span><span class="sxs-lookup"><span data-stu-id="5c781-145">Modify the **publicDomainName** parameter.</span></span> <span data-ttu-id="5c781-146">Detta blir en del av DNS-namnet som associeras med load balancer offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="5c781-146">This will become part of the DNS name associated with the load balancer public IP.</span></span> <span data-ttu-id="5c781-147">Den slutliga FQDN har formatet för *[värdet för den här parametern]*. *[region]* . cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="5c781-147">The final FQDN will have the format of *[value of this parameter]*.*[region]*.cloudapp.azure.com.</span></span> <span data-ttu-id="5c781-148">Till exempel om du anger namnet som deishbai32 och resursgruppen har distribuerats till regionen västra USA, blir sedan sista FQDN till din belastningsutjämnare deishbai32.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="5c781-148">For example, if you specify the name as deishbai32, and the resource group is deployed to the West US region, then the final FQDN to your load balancer will be deishbai32.westus.cloudapp.azure.com.</span></span>
10. <span data-ttu-id="5c781-149">Spara parameterfilen med.</span><span class="sxs-lookup"><span data-stu-id="5c781-149">Save the parameter file.</span></span> <span data-ttu-id="5c781-150">Och sedan kan du etablera klustret med Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5c781-150">And then you can provision the cluster using Azure PowerShell:</span></span>
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    <span data-ttu-id="5c781-151">eller Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="5c781-151">or Azure CLI:</span></span>
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. <span data-ttu-id="5c781-152">När resursgruppen har etablerats kan du se alla resurser i gruppen på klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5c781-152">Once the resource group is provisioned, you can see all the resources in the group on Azure classic portal.</span></span> <span data-ttu-id="5c781-153">I följande skärmbild visas resursgruppen innehåller ett virtuellt nätverk med tre virtuella datorer, som är anslutna till samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="5c781-153">As shown in the following screenshot, the resource group contains a virtual network with three VMs, which are joined to the same availability set.</span></span> <span data-ttu-id="5c781-154">Gruppen innehåller också en belastningsutjämnare som har en tillhörande offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5c781-154">The group also contains a load balancer, which has an associated public IP.</span></span>
    
    ![Etablerade resursgruppen på den klassiska Azure-portalen](./media/deis-cluster/resource-group.png)

## <a name="install-the-client"></a><span data-ttu-id="5c781-156">Installera klienten</span><span class="sxs-lookup"><span data-stu-id="5c781-156">Install the client</span></span>
<span data-ttu-id="5c781-157">Du behöver **deisctl** att kontrollera din Deis klustret.</span><span class="sxs-lookup"><span data-stu-id="5c781-157">You need **deisctl** to control your Deis cluster.</span></span> <span data-ttu-id="5c781-158">Även om deisctl installeras automatiskt på alla noder i klustret, är det en bra idé att använda deisctl på en separat administrativ dator.</span><span class="sxs-lookup"><span data-stu-id="5c781-158">Although deisctl is automatically installed in all the cluster nodes, it's a good practice to use deisctl on a separate administrative machine.</span></span> <span data-ttu-id="5c781-159">Eftersom alla noder har konfigurerats med endast privata IP-adresser behöver du dessutom använda SSH-tunnel genom belastningsutjämnare, som har en offentlig IP-adress, att ansluta till noden-datorer.</span><span class="sxs-lookup"><span data-stu-id="5c781-159">Furthermore, because all nodes are configured with only private IP addresses, you'll need to use SSH tunneling through the load balancer, which has a public IP, to connect to the node machines.</span></span> <span data-ttu-id="5c781-160">Nedan följer stegen för att konfigurera deisctl på en separat Ubuntu fysiska eller virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5c781-160">The following are the steps of setting up deisctl on a separate Ubuntu physical or virtual machine.</span></span>

1. <span data-ttu-id="5c781-161">Installera deisctl:mkdir deis</span><span class="sxs-lookup"><span data-stu-id="5c781-161">Install deisctl:mkdir deis</span></span>
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. <span data-ttu-id="5c781-162">Lägg till din privata nyckel till ssh agent:</span><span class="sxs-lookup"><span data-stu-id="5c781-162">Add your private key to ssh agent:</span></span>
   
        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]
3. <span data-ttu-id="5c781-163">Konfigurera deisctl:</span><span class="sxs-lookup"><span data-stu-id="5c781-163">Configure deisctl:</span></span>
   
        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

<span data-ttu-id="5c781-164">Mallen definierar ingående NAT-regler som mappar 2223 till instansen 1, 2224 till 2-instans och 2225 till 3-instans.</span><span class="sxs-lookup"><span data-stu-id="5c781-164">The template defines inbound NAT rules that map 2223 to instance 1, 2224 to instance 2, and 2225 to instance 3.</span></span> <span data-ttu-id="5c781-165">Detta ger redundans för att använda verktyget deisctl.</span><span class="sxs-lookup"><span data-stu-id="5c781-165">This provides redundancy for using the deisctl tool.</span></span> <span data-ttu-id="5c781-166">Du kan granska reglerna på den klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="5c781-166">You can examine these rules on Azure classic portal:</span></span>

![NAT-regler på belastningsutjämnaren](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> <span data-ttu-id="5c781-168">Mallen stöder för närvarande endast kluster 3-noder.</span><span class="sxs-lookup"><span data-stu-id="5c781-168">Currently the template only supports 3-node clusters.</span></span> <span data-ttu-id="5c781-169">Detta är på grund av en begränsning i Azure Resource Manager-mall NAT Regeldefinitionen som inte stöder loop-syntax.</span><span class="sxs-lookup"><span data-stu-id="5c781-169">This is because of a limitation in Azure Resource Manager template NAT rule definition, which doesn’t support loop syntax.</span></span>
> 
> 

## <a name="install-and-start-the-deis-platform"></a><span data-ttu-id="5c781-170">Installera och starta den Deis plattform</span><span class="sxs-lookup"><span data-stu-id="5c781-170">Install and start the Deis platform</span></span>
<span data-ttu-id="5c781-171">Nu kan du använda deisctl för att installera och starta den Deis plattform:</span><span class="sxs-lookup"><span data-stu-id="5c781-171">Now you can use deisctl to install and start the Deis platform:</span></span>

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> <span data-ttu-id="5c781-172">Startar plattformen som tar ett tag (upp till 10 minuter).</span><span class="sxs-lookup"><span data-stu-id="5c781-172">Starting the platform takes a while (as much as 10 minutes).</span></span> <span data-ttu-id="5c781-173">Särskilt, kan startar tjänsten builder ta lång tid.</span><span class="sxs-lookup"><span data-stu-id="5c781-173">Especially, starting the builder service can take a long time.</span></span> <span data-ttu-id="5c781-174">Och ibland tar det lite tid ska lyckas: om åtgärden verkar låser sig, försök att skriva `ctrl+c` bryta körning av kommandot och försök sedan igen.</span><span class="sxs-lookup"><span data-stu-id="5c781-174">And sometimes it takes a few tries to succeed: If the operation seems to hang, try typing `ctrl+c` to break execution of the command and retry.</span></span>
> 
> 

<span data-ttu-id="5c781-175">Du kan använda `deisctl list` att kontrollera om alla tjänster körs:</span><span class="sxs-lookup"><span data-stu-id="5c781-175">You can use `deisctl list` to verify if all services are running:</span></span>

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

<span data-ttu-id="5c781-176">Grattis!</span><span class="sxs-lookup"><span data-stu-id="5c781-176">Congratulations!</span></span> <span data-ttu-id="5c781-177">Nu har du en löpande Deis clsuter i Azure!</span><span class="sxs-lookup"><span data-stu-id="5c781-177">Now you've got a running Deis clsuter on Azure!</span></span> <span data-ttu-id="5c781-178">Nu ska vi distribuerar ett exempel gå program för att se klustret i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="5c781-178">Next, let's deploy a sample Go application to see the cluster in action.</span></span>

## <a name="deploy-and-scale-a-hello-world-application"></a><span data-ttu-id="5c781-179">Distribuera och skala en programmet Hello World</span><span class="sxs-lookup"><span data-stu-id="5c781-179">Deploy and scale a Hello World application</span></span>
<span data-ttu-id="5c781-180">Följande steg visar hur du distribuerar en ”Hello World” gå program i klustret.</span><span class="sxs-lookup"><span data-stu-id="5c781-180">The following steps show how to deploy a "Hello World" Go application to the cluster.</span></span> <span data-ttu-id="5c781-181">Anvisningarna är baserade på [Deis dokumentationen](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span><span class="sxs-lookup"><span data-stu-id="5c781-181">The steps are based on [Deis documentation](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span></span>

1. <span data-ttu-id="5c781-182">För routning nät ska fungera korrekt måste ha ett jokertecken A-posten för din domän som pekar på offentliga IP-Adressen för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="5c781-182">For the routing mesh to work properly, you’ll need to have a wildcard A record for your domain pointing to the public IP of the load balancer.</span></span> <span data-ttu-id="5c781-183">Följande skärmbild visar A-post för en exempel-domänregistrering på GoDaddy:</span><span class="sxs-lookup"><span data-stu-id="5c781-183">The following screenshot shows the A record for a sample domain registration on GoDaddy:</span></span>
   
    ![GoDaddy A-post](./media/deis-cluster/go-daddy.png)
   
   <p />
2. <span data-ttu-id="5c781-185">Installera deis:</span><span class="sxs-lookup"><span data-stu-id="5c781-185">Install deis:</span></span>
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. <span data-ttu-id="5c781-186">Skapa en ny SSH-nyckel och Lägg sedan till den offentliga nyckeln till GitHub (naturligtvis kan du också återanvända dina befintliga nycklar).</span><span class="sxs-lookup"><span data-stu-id="5c781-186">Create a new SSH key, and then add the public key to GitHub (of course, you can also reuse your existing keys).</span></span> <span data-ttu-id="5c781-187">Använd för att skapa en ny SSH-nyckel:</span><span class="sxs-lookup"><span data-stu-id="5c781-187">To create a new SSH key pair, use:</span></span>
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)
4. <span data-ttu-id="5c781-188">Lägga till id_rsa.pub eller den offentliga nyckeln för ditt val i GitHub.</span><span class="sxs-lookup"><span data-stu-id="5c781-188">Add id_rsa.pub, or the public key of your choice, to GitHub.</span></span> <span data-ttu-id="5c781-189">Du kan göra detta med hjälp av Lägg till SSH key knappen på din skärm för SSH-nycklar konfiguration:</span><span class="sxs-lookup"><span data-stu-id="5c781-189">You can do this by using the Add SSH key button in your SSH keys configuration screen:</span></span>
   
   ![GitHub-nyckel](./media/deis-cluster/github-key.png)
   
   <p />
5. <span data-ttu-id="5c781-191">Registrera en ny användare:</span><span class="sxs-lookup"><span data-stu-id="5c781-191">Register a new user:</span></span>
   
        deis register http://deis.[your domain]
   <p />
6. <span data-ttu-id="5c781-192">Lägg till SSH-nyckeln:</span><span class="sxs-lookup"><span data-stu-id="5c781-192">Add the SSH key:</span></span>
   
        deis keys:add [path to your SSH public key]
   <p />      
7. <span data-ttu-id="5c781-193">Skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="5c781-193">Create an application.</span></span>
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p /><span data-ttu-id="5c781-194">
8.Git push utlöser Docker avbildningar för skapats och distribuerats, vilket kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="5c781-194">
8. The git push will trigger Docker images to be built and deployed, which will take a few minutes.</span></span> <span data-ttu-id="5c781-195">Från min erfarenhet ibland låser steg 10 (Pushing bild privata databasen).</span><span class="sxs-lookup"><span data-stu-id="5c781-195">From my experience, occasionally, Step 10 (Pushing image to private repository) may hang.</span></span> <span data-ttu-id="5c781-196">När detta inträffar kan du avbryta processen, ta bort programmet med hjälp av ' deis appar: förstör – a <application name> ` to remove the application and try again. You can use `deis apps:list' Ta reda på namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="5c781-196">When this happens, you can stop the process, remove the application using `deis apps:destroy –a <application name>` to remove the application and try again. You can use `deis apps:list` to find out the name of your application.</span></span> <span data-ttu-id="5c781-197">Om allt fungerar bör du se något som liknar följande i slutet av kommandoutdata:</span><span class="sxs-lookup"><span data-stu-id="5c781-197">If everything works out, you should see something like the following at the end of command outputs:</span></span>
   
        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. <span data-ttu-id="5c781-198">Kontrollera att programmet fungerar:</span><span class="sxs-lookup"><span data-stu-id="5c781-198">Verify if the application is working:</span></span>
   
        curl -S http://[your application name].[your domain]
   <span data-ttu-id="5c781-199">Du bör se:</span><span class="sxs-lookup"><span data-stu-id="5c781-199">You should see:</span></span>
   
        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
   <p />
10. <span data-ttu-id="5c781-200">Skala program till 3 instanser:</span><span class="sxs-lookup"><span data-stu-id="5c781-200">Scale the application to 3 instances:</span></span>
    
        deis scale cmd=3
    <p />
11. <span data-ttu-id="5c781-201">Du kan använda deis information om du vill granska information om ditt program.</span><span class="sxs-lookup"><span data-stu-id="5c781-201">Optionally, you can use deis info to examine details of your application.</span></span> <span data-ttu-id="5c781-202">Följande utdata är från min programdistributionen:</span><span class="sxs-lookup"><span data-stu-id="5c781-202">The following outputs are from my application deployment:</span></span>
    
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

## <a name="next-steps"></a><span data-ttu-id="5c781-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c781-203">Next Steps</span></span>
<span data-ttu-id="5c781-204">Den här artikeln gått du igenom alla stegen för att etablera en ny Deis kluster på Azure med hjälp av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="5c781-204">This article walked you through all the steps to provision a new Deis cluster on Azure using an Azure Resource Manager template.</span></span> <span data-ttu-id="5c781-205">Mallen har stöd för redundans i tooling anslutningar samt belastningsutjämning för distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="5c781-205">The template supports redundancy in tooling connections as well as load balancing for deployed applications.</span></span> <span data-ttu-id="5c781-206">Mallen undviker du också använda offentliga IP-adresser för medlem noder, vilket sparar värdefull offentliga IP-resurser och ger en säkrare miljö som värd för program.</span><span class="sxs-lookup"><span data-stu-id="5c781-206">The template also avoids using public IPs on member nodes, which saves precious public IP resources and provides a more secured environment to host applications.</span></span> <span data-ttu-id="5c781-207">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="5c781-207">To learn more, see the following articles:</span></span>

<span data-ttu-id="5c781-208">[Översikt över Azure Resource Manager][resource-group-overview]</span><span class="sxs-lookup"><span data-stu-id="5c781-208">[Azure Resource Manager Overview][resource-group-overview]</span></span>  
<span data-ttu-id="5c781-209">[Hur du använder Azure CLI][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="5c781-209">[How to use the Azure CLI][azure-command-line-tools]</span></span>  
<span data-ttu-id="5c781-210">[Använda Azure PowerShell med Azure Resource Manager][powershell-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="5c781-210">[Using Azure PowerShell with Azure Resource Manager][powershell-azure-resource-manager]</span></span>  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
