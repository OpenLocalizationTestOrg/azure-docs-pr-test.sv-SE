---
title: "Skapa ett fristående kluster med Azure virtuella datorer som kör Windows | Microsoft Docs"
description: "Lär dig hur du skapar och hanterar Azure Service Fabric-kluster på Azure virtuella datorer som kör Windows Server."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 7eeb40d2-fb22-4a77-80ca-f1b46b22edbc
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: ryanwi;chackdan
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: f8a0305a22c00f9bdbdb1bdb06dc299cccee23dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a><span data-ttu-id="6a0d0-103">Skapa en tre noder fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows Server</span><span class="sxs-lookup"><span data-stu-id="6a0d0-103">Create a three node standalone Service Fabric cluster with Azure virtual machines running Windows Server</span></span>
<span data-ttu-id="6a0d0-104">Den här artikeln beskriver hur du skapar ett kluster på Windows-baserade virtuella Azure-datorer (VM) med fristående Service Fabric-installationsprogram för Windows Server.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-104">This article describes how to create a cluster on Windows-based Azure virtual machines (VMs), using the standalone Service Fabric installer for Windows Server.</span></span> <span data-ttu-id="6a0d0-105">Scenariot är ett specialfall av [skapa och hantera ett kluster som körs på Windows Server](service-fabric-cluster-creation-for-windows-server.md) där de virtuella datorerna är [Azure virtuella datorer som kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), men du inte skapar [en Azure molnbaserad Service Fabric-kluster](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6a0d0-105">The scenario is a special case of [Create and manage a cluster running on Windows Server](service-fabric-cluster-creation-for-windows-server.md) where the VMs are [Azure VMs running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), however you are not creating [an Azure cloud-based Service Fabric cluster](service-fabric-cluster-creation-via-portal.md).</span></span> <span data-ttu-id="6a0d0-106">Skillnad i följande det här mönstret är att fristående Service Fabric-kluster skapas med följande steg hanteras helt av dig, medan Azure molnbaserade Service Fabric klustren hanteras och uppgraderas av Service Fabric-resurs providern.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-106">The distinction in following this pattern is that the standalone Service Fabric cluster created by the following steps is entirely managed by you, whereas the Azure cloud-based Service Fabric clusters are managed and upgraded by the Service Fabric resource provider.</span></span>

## <a name="steps-to-create-the-standalone-cluster"></a><span data-ttu-id="6a0d0-107">Steg för att skapa fristående klustret</span><span class="sxs-lookup"><span data-stu-id="6a0d0-107">Steps to create the standalone cluster</span></span>
1. <span data-ttu-id="6a0d0-108">Logga in på Azure-portalen och skapa en ny Windows Server 2012 R2 Datacenter eller Windows Server 2016 Datacenter VM i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-108">Sign in to the Azure portal and create a new Windows Server 2012 R2 Datacenter or Windows Server 2016 Datacenter VM in a resource group.</span></span> <span data-ttu-id="6a0d0-109">Läs artikeln [skapa en virtuell Windows-dator i Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-109">Read the article [Create a Windows VM in the Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for more details.</span></span>
2. <span data-ttu-id="6a0d0-110">Lägga till några fler virtuella datorer i samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-110">Add a couple more VMs to the same resource group.</span></span> <span data-ttu-id="6a0d0-111">Kontrollera att var och en av de virtuella datorerna har samma administratörsanvändarnamn och lösenord när de skapas.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-111">Ensure that each of the VMs has the same administrator user name and password when created.</span></span> <span data-ttu-id="6a0d0-112">En gång skapade du bör se alla tre virtuella datorer i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-112">Once created you should see all three VMs in the same virtual network.</span></span>
3. <span data-ttu-id="6a0d0-113">Ansluta till var och en av de virtuella datorerna och inaktivera Windows-brandväggen med den [Serverhanteraren instrumentpanelen lokal Server](https://technet.microsoft.com/library/jj134147.aspx).</span><span class="sxs-lookup"><span data-stu-id="6a0d0-113">Connect to each of the VMs and turn off the Windows Firewall using the [Server Manager, Local Server dashboard](https://technet.microsoft.com/library/jj134147.aspx).</span></span> <span data-ttu-id="6a0d0-114">Detta säkerställer att nätverkstrafik kan kommunicera mellan datorerna.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-114">This ensures that the network traffic can communicate between the machines.</span></span> <span data-ttu-id="6a0d0-115">När du är ansluten till varje dator hämta IP-adress genom att öppna en kommandotolk och skriva `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-115">While connected to each machine, get the IP address by opening a command prompt and typing `ipconfig`.</span></span> <span data-ttu-id="6a0d0-116">Du kan också se IP-adressen för varje dator på portalen genom att välja resursen virtuellt nätverk för resursgruppen och kontrollera nätverksgränssnitt som skapats för var och en av dessa datorer.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-116">Alternatively you can see the IP address of each machine on the portal, by selecting the virtual network resource for the resource group and checking the network interfaces created for each of these machines.</span></span>
4. <span data-ttu-id="6a0d0-117">Ansluta till en av de virtuella datorerna och testa att du kan pinga har de andra två virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-117">Connect to one of the VMs and test that you can ping the other two VMs successfully.</span></span>
5. <span data-ttu-id="6a0d0-118">Ansluta till en av de virtuella datorerna och [hämta fristående Service Fabric-paket för Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) till en ny mapp på datorn och extrahera paketet.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-118">Connect to one of the VMs and [download the standalone Service Fabric package for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) into a new folder on the machine and extract the package.</span></span>
6. <span data-ttu-id="6a0d0-119">Öppna den *ClusterConfig.Unsecure.MultiMachine.json* filen i anteckningar och redigera varje nod med tre IP-adresser för datorer.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-119">Open the *ClusterConfig.Unsecure.MultiMachine.json* file in Notepad and edit each node with the three IP addresses of the machines.</span></span> <span data-ttu-id="6a0d0-120">Ändra klusternamnet överst och spara filen.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-120">Change the cluster name at the top and save the file.</span></span>  <span data-ttu-id="6a0d0-121">En partiell exempel på klustermanifestet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-121">A partial example of the cluster manifest is shown below.</span></span>
   
    ```
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "01-2017",
        "nodes": [
        {
            "nodeName": "standalonenode0",
            "iPAddress": "10.1.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "standalonenode1",
            "iPAddress": "10.1.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "standalonenode2",
            "iPAddress": "10.1.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
        ],
    ```
7. <span data-ttu-id="6a0d0-122">Om du vill att detta ska vara en säker, besluta säkerhetsåtgärd som du vill använda och följ stegen i den associerade länken: [X509 certifikat](service-fabric-windows-cluster-x509-security.md) eller [Windows-säkerhet](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="6a0d0-122">If you intend this to be a secure cluster, decide the security measure you would like to use and follow the steps at the associated link: [X509 Certificate](service-fabric-windows-cluster-x509-security.md) or [Windows Security](service-fabric-windows-cluster-windows-security.md).</span></span> <span data-ttu-id="6a0d0-123">Om klustret med Windows-säkerhet, behöver du konfigurera en domänkontrollant för att hantera Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-123">If setting up the cluster using Windows Security, you will need to set up a domain controller to manage Active Directory.</span></span> <span data-ttu-id="6a0d0-124">Observera att en domänkontrollant dator som en Service Fabric nod inte stöds.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-124">Note that using a domain controller machine as a Service Fabric node is not supported.</span></span>
8. <span data-ttu-id="6a0d0-125">Öppna en [fönstret PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span><span class="sxs-lookup"><span data-stu-id="6a0d0-125">Open a [PowerShell ISE window](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span></span> <span data-ttu-id="6a0d0-126">Navigera till mappen där du extraherade installationspaketet hämtade fristående och spara konfigurationsfilen för klustret.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-126">Navigate to the folder where you extracted the downloaded standalone installer package and saved the cluster configuration file.</span></span> <span data-ttu-id="6a0d0-127">Kör följande PowerShell-kommando för att distribuera klustret:</span><span class="sxs-lookup"><span data-stu-id="6a0d0-127">Run the following PowerShell command to deploy the cluster:</span></span>
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

<span data-ttu-id="6a0d0-128">Skriptet fjärrkonfigurera Service Fabric-kluster och bör rapportera förlopp som distributionen samlar via.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-128">The script will remotely configure the Service Fabric cluster and should report progress as deployment rolls through.</span></span>

9. <span data-ttu-id="6a0d0-129">Efter ungefär en minut kan du kontrollera om klustret fungerar genom att ansluta till Service Fabric-Utforskaren med ett av datorns IP-adresser, till exempel med hjälp av `http://10.1.0.5:19080/Explorer/index.html`.</span><span class="sxs-lookup"><span data-stu-id="6a0d0-129">After about a minute, you can check if the cluster is operational by connecting to the Service Fabric Explorer using one of the machine's IP addresses, for example by using `http://10.1.0.5:19080/Explorer/index.html`.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6a0d0-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a0d0-130">Next steps</span></span>
* [<span data-ttu-id="6a0d0-131">Skapa fristående Service Fabric-kluster i Windows Server eller Linux</span><span class="sxs-lookup"><span data-stu-id="6a0d0-131">Create standalone Service Fabric clusters on Windows Server or Linux</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="6a0d0-132">Lägg till eller ta bort noder i ett fristående Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="6a0d0-132">Add or remove nodes to a standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="6a0d0-133">Konfigurationsinställningar för fristående Windows-kluster</span><span class="sxs-lookup"><span data-stu-id="6a0d0-133">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="6a0d0-134">Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet</span><span class="sxs-lookup"><span data-stu-id="6a0d0-134">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="6a0d0-135">Skydda ett fristående kluster på Windows med X509 certifikat</span><span class="sxs-lookup"><span data-stu-id="6a0d0-135">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

