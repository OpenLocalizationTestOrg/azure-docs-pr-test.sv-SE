---
title: "aaaCreate en fristående kluster med Azure virtuella datorer som kör Windows | Microsoft Docs"
description: "Lär dig hur toocreate och hantera en Azure Service Fabric-kluster på Azure virtuella datorer som kör Windows Server."
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
ms.openlocfilehash: 8900204fe69887a7a0ca54b06e0d32534421bcfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a><span data-ttu-id="7f48c-103">Skapa en tre noder fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows Server</span><span class="sxs-lookup"><span data-stu-id="7f48c-103">Create a three node standalone Service Fabric cluster with Azure virtual machines running Windows Server</span></span>
<span data-ttu-id="7f48c-104">Den här artikeln beskriver hur toocreate ett kluster på Windows-baserade virtuella Azure-datorer (VM) med hjälp av hello fristående Service Fabric-installationsprogram för Windows Server.</span><span class="sxs-lookup"><span data-stu-id="7f48c-104">This article describes how toocreate a cluster on Windows-based Azure virtual machines (VMs), using hello standalone Service Fabric installer for Windows Server.</span></span> <span data-ttu-id="7f48c-105">hello scenario är ett specialfall av [skapa och hantera ett kluster som körs på Windows Server](service-fabric-cluster-creation-for-windows-server.md) där hello är [Azure virtuella datorer som kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), men du inte skapar [en Azure molnbaserad Service Fabric-kluster](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7f48c-105">hello scenario is a special case of [Create and manage a cluster running on Windows Server](service-fabric-cluster-creation-for-windows-server.md) where hello VMs are [Azure VMs running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), however you are not creating [an Azure cloud-based Service Fabric cluster](service-fabric-cluster-creation-via-portal.md).</span></span> <span data-ttu-id="7f48c-106">hello skillnad i följande det här mönstret är att hello fristående Service Fabric-kluster som skapats av hello följande hanteras helt av dig, medan hello Azure molnbaserade Service Fabric-kluster hanteras och uppgraderas av hello Service Fabric resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="7f48c-106">hello distinction in following this pattern is that hello standalone Service Fabric cluster created by hello following steps is entirely managed by you, whereas hello Azure cloud-based Service Fabric clusters are managed and upgraded by hello Service Fabric resource provider.</span></span>

## <a name="steps-toocreate-hello-standalone-cluster"></a><span data-ttu-id="7f48c-107">Steg toocreate hello fristående kluster</span><span class="sxs-lookup"><span data-stu-id="7f48c-107">Steps toocreate hello standalone cluster</span></span>
1. <span data-ttu-id="7f48c-108">Logga in toohello Azure-portalen och skapa en ny Windows Server 2012 R2 Datacenter eller Windows Server 2016 Datacenter VM i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f48c-108">Sign in toohello Azure portal and create a new Windows Server 2012 R2 Datacenter or Windows Server 2016 Datacenter VM in a resource group.</span></span> <span data-ttu-id="7f48c-109">Läs hello artikel [skapa en virtuell Windows-dator i hello Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för mer information.</span><span class="sxs-lookup"><span data-stu-id="7f48c-109">Read hello article [Create a Windows VM in hello Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for more details.</span></span>
2. <span data-ttu-id="7f48c-110">Lägga till några fler virtuella datorer toohello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f48c-110">Add a couple more VMs toohello same resource group.</span></span> <span data-ttu-id="7f48c-111">Se till att varje hello virtuella datorer har hello samma administratörsanvändarnamn och lösenord när de skapas.</span><span class="sxs-lookup"><span data-stu-id="7f48c-111">Ensure that each of hello VMs has hello same administrator user name and password when created.</span></span> <span data-ttu-id="7f48c-112">En gång skapade du bör se alla tre virtuella datorer i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f48c-112">Once created you should see all three VMs in hello same virtual network.</span></span>
3. <span data-ttu-id="7f48c-113">Ansluta tooeach av hello virtuella datorer och inaktiverar hello Windows-brandväggen med hjälp av hello [Serverhanteraren instrumentpanelen lokal Server](https://technet.microsoft.com/library/jj134147.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f48c-113">Connect tooeach of hello VMs and turn off hello Windows Firewall using hello [Server Manager, Local Server dashboard](https://technet.microsoft.com/library/jj134147.aspx).</span></span> <span data-ttu-id="7f48c-114">Detta säkerställer att hello nätverkstrafik kan kommunicera mellan hello datorer.</span><span class="sxs-lookup"><span data-stu-id="7f48c-114">This ensures that hello network traffic can communicate between hello machines.</span></span> <span data-ttu-id="7f48c-115">När anslutna tooeach datorn hämta hello IP-adress genom att öppna en kommandotolk och skriva `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="7f48c-115">While connected tooeach machine, get hello IP address by opening a command prompt and typing `ipconfig`.</span></span> <span data-ttu-id="7f48c-116">Du kan också se hello IP-adressen för varje dator på hello-portalen genom att välja hello virtuella nätverksresurs för hello resursgrupp och kontrollerar hello nätverksgränssnitt som skapats för var och en av dessa datorer.</span><span class="sxs-lookup"><span data-stu-id="7f48c-116">Alternatively you can see hello IP address of each machine on hello portal, by selecting hello virtual network resource for hello resource group and checking hello network interfaces created for each of these machines.</span></span>
4. <span data-ttu-id="7f48c-117">Anslut tooone av hello virtuella datorer och kontrollera att du kan pinga hello andra två virtuella datorer har.</span><span class="sxs-lookup"><span data-stu-id="7f48c-117">Connect tooone of hello VMs and test that you can ping hello other two VMs successfully.</span></span>
5. <span data-ttu-id="7f48c-118">Ansluta tooone av hello virtuella datorer och [hämta hello fristående Service Fabric-paket för Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) till en ny mapp på hello datorn och extrahera hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="7f48c-118">Connect tooone of hello VMs and [download hello standalone Service Fabric package for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) into a new folder on hello machine and extract hello package.</span></span>
6. <span data-ttu-id="7f48c-119">Öppna hello *ClusterConfig.Unsecure.MultiMachine.json* filen i anteckningar och redigera varje nod med hello tre IP-adresser hello datorer.</span><span class="sxs-lookup"><span data-stu-id="7f48c-119">Open hello *ClusterConfig.Unsecure.MultiMachine.json* file in Notepad and edit each node with hello three IP addresses of hello machines.</span></span> <span data-ttu-id="7f48c-120">Ändra hello klusternamnet hello överst och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="7f48c-120">Change hello cluster name at hello top and save hello file.</span></span>  <span data-ttu-id="7f48c-121">En partiell exempel på hello klustermanifestet visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7f48c-121">A partial example of hello cluster manifest is shown below.</span></span>
   
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
7. <span data-ttu-id="7f48c-122">Om du planerar denna toobe säker kluster, bestämma hello säkerhetsåtgärd du gillar toouse och följ anvisningarna hello i hello associerade länk: [X509 certifikat](service-fabric-windows-cluster-x509-security.md) eller [Windows-säkerhet](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="7f48c-122">If you intend this toobe a secure cluster, decide hello security measure you would like toouse and follow hello steps at hello associated link: [X509 Certificate](service-fabric-windows-cluster-x509-security.md) or [Windows Security](service-fabric-windows-cluster-windows-security.md).</span></span> <span data-ttu-id="7f48c-123">Om du ställer in hello-kluster med hjälp av Windows-säkerhet behöver tooset upp en domain controller toomanage Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7f48c-123">If setting up hello cluster using Windows Security, you will need tooset up a domain controller toomanage Active Directory.</span></span> <span data-ttu-id="7f48c-124">Observera att en domänkontrollant dator som en Service Fabric nod inte stöds.</span><span class="sxs-lookup"><span data-stu-id="7f48c-124">Note that using a domain controller machine as a Service Fabric node is not supported.</span></span>
8. <span data-ttu-id="7f48c-125">Öppna en [fönstret PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span><span class="sxs-lookup"><span data-stu-id="7f48c-125">Open a [PowerShell ISE window](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span></span> <span data-ttu-id="7f48c-126">Navigera toohello mappen där du extraherade hello hämtade fristående installer-paketet och sparat hello konfigurationsfilen för klustret.</span><span class="sxs-lookup"><span data-stu-id="7f48c-126">Navigate toohello folder where you extracted hello downloaded standalone installer package and saved hello cluster configuration file.</span></span> <span data-ttu-id="7f48c-127">Kör följande PowerShell-kommandot toodeploy hello klustret hello:</span><span class="sxs-lookup"><span data-stu-id="7f48c-127">Run hello following PowerShell command toodeploy hello cluster:</span></span>
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

<span data-ttu-id="7f48c-128">hello skriptet fjärrkonfigurera hello Service Fabric-kluster och bör rapportera förlopp som distributionen samlar via.</span><span class="sxs-lookup"><span data-stu-id="7f48c-128">hello script will remotely configure hello Service Fabric cluster and should report progress as deployment rolls through.</span></span>

9. <span data-ttu-id="7f48c-129">Efter ungefär en minut kan du kontrollera om hello klustret fungerar genom att ansluta toohello Service Fabric Explorer med någon av hello dator-IP-adresser, till exempel med hjälp av `http://10.1.0.5:19080/Explorer/index.html`.</span><span class="sxs-lookup"><span data-stu-id="7f48c-129">After about a minute, you can check if hello cluster is operational by connecting toohello Service Fabric Explorer using one of hello machine's IP addresses, for example by using `http://10.1.0.5:19080/Explorer/index.html`.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7f48c-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f48c-130">Next steps</span></span>
* [<span data-ttu-id="7f48c-131">Skapa fristående Service Fabric-kluster i Windows Server eller Linux</span><span class="sxs-lookup"><span data-stu-id="7f48c-131">Create standalone Service Fabric clusters on Windows Server or Linux</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="7f48c-132">Lägg till eller ta bort noder tooa fristående Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="7f48c-132">Add or remove nodes tooa standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="7f48c-133">Konfigurationsinställningar för fristående Windows-kluster</span><span class="sxs-lookup"><span data-stu-id="7f48c-133">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="7f48c-134">Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet</span><span class="sxs-lookup"><span data-stu-id="7f48c-134">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="7f48c-135">Skydda ett fristående kluster på Windows med X509 certifikat</span><span class="sxs-lookup"><span data-stu-id="7f48c-135">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

