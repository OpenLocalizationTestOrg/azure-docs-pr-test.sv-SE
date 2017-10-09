---
title: aaaSet in en Linux RDMA klustret toorun MPI program | Microsoft Docs
description: "Skapa ett Linux-kluster i storlek H16r H16mr, A8 eller A9 VMs toouse hello Azure RDMA nätverk toorun MPI appar"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 3199317a37b095e80718d6724954687d30aea3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-linux-rdma-cluster-toorun-mpi-applications"></a><span data-ttu-id="862b3-103">Ställa in en Linux RDMA klustret toorun MPI-program</span><span class="sxs-lookup"><span data-stu-id="862b3-103">Set up a Linux RDMA cluster toorun MPI applications</span></span>
<span data-ttu-id="862b3-104">Lär dig hur tooset in RDMA en Linux-kluster i Azure med [högpresterande compute VM-storlekar](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun parallella Message Passing Interface (MPI) program.</span><span class="sxs-lookup"><span data-stu-id="862b3-104">Learn how tooset up a Linux RDMA cluster in Azure with [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun parallel Message Passing Interface (MPI) applications.</span></span> <span data-ttu-id="862b3-105">Den här artikeln innehåller steg tooprepare en Linux HPC avbildningen toorun Intel MPI på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-105">This article provides steps tooprepare a Linux HPC image toorun Intel MPI on a cluster.</span></span> <span data-ttu-id="862b3-106">Efter förberedelse, kan du distribuera ett kluster för virtuella datorer med hjälp av den här avbildningen och en av hello RDMA-kompatibla Azure VM-storlekar (för närvarande H16r H16mr, A8 eller A9).</span><span class="sxs-lookup"><span data-stu-id="862b3-106">After preparation, you deploy a cluster of VMs using this image and one of hello RDMA-capable Azure VM sizes (currently H16r, H16mr, A8, or A9).</span></span> <span data-ttu-id="862b3-107">Använd hello klustret toorun MPI program som effektiv kommunicerar över låg latens, hög genomströmning nätverk som baseras på direktåtkomst till fjärrminne (RDMA)-teknik.</span><span class="sxs-lookup"><span data-stu-id="862b3-107">Use hello cluster toorun MPI applications that communicate efficiently over a low-latency, high-throughput network based on remote direct memory access (RDMA) technology.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="862b3-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk.</span><span class="sxs-lookup"><span data-stu-id="862b3-108">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="862b3-109">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="862b3-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="862b3-110">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="862b3-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

## <a name="cluster-deployment-options"></a><span data-ttu-id="862b3-111">Distributionsalternativ för kluster</span><span class="sxs-lookup"><span data-stu-id="862b3-111">Cluster deployment options</span></span>
<span data-ttu-id="862b3-112">Följande är metoder du kan använda toocreate ett Linux RDMA-kluster med eller utan en jobbschemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="862b3-112">Following are methods you can use toocreate a Linux RDMA cluster with or without a job scheduler.</span></span>

* <span data-ttu-id="862b3-113">**Azure CLI-skript**: enligt nedan använder hello [Azure-kommandoradsgränssnittet](../../../cli-install-nodejs.md) (CLI) tooscript hello distributionen av ett kluster med RDMA-kompatibla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="862b3-113">**Azure CLI scripts**: As shown later in this article, use hello [Azure command-line interface](../../../cli-install-nodejs.md) (CLI) tooscript hello deployment of a cluster of RDMA-capable VMs.</span></span> <span data-ttu-id="862b3-114">hello CLI i Service Management-läge skapar hello klusternoder följd i hello klassiska distributionsmodellen så distribuerar många compute-noder kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="862b3-114">hello CLI in Service Management mode creates hello cluster nodes serially in hello classic deployment model, so deploying many compute nodes might take several minutes.</span></span> <span data-ttu-id="862b3-115">tooenable hello RDMA-nätverksanslutning när du använder hello klassiska distributionsmodellen distribuera hello virtuella datorer i hello samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="862b3-115">tooenable hello RDMA network connection when you use hello classic deployment model, deploy hello VMs in hello same cloud service.</span></span>
* <span data-ttu-id="862b3-116">**Azure Resource Manager-mallar**: du kan också använda hello Resource Manager distribution modellen toodeploy ett kluster med RDMA-kompatibla virtuella datorer som ansluter toohello RDMA.</span><span class="sxs-lookup"><span data-stu-id="862b3-116">**Azure Resource Manager templates**: You can also use hello Resource Manager deployment model toodeploy a cluster of RDMA-capable VMs that connects toohello RDMA network.</span></span> <span data-ttu-id="862b3-117">Du kan [skapa en egen mall](../../../resource-group-authoring-templates.md), eller kontrollera hello [Azure quickstart mallar](https://azure.microsoft.com/documentation/templates/) för mallar som har bidragit med Microsoft eller hello community toodeploy hello lösningen som du söker.</span><span class="sxs-lookup"><span data-stu-id="862b3-117">You can [create your own template](../../../resource-group-authoring-templates.md), or check hello [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/) for templates contributed by Microsoft or hello community toodeploy hello solution you want.</span></span> <span data-ttu-id="862b3-118">Resource Manager-mallar kan ge en snabb och tillförlitlig sätt toodeploy ett Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-118">Resource Manager templates can provide a fast and reliable way toodeploy a Linux cluster.</span></span> <span data-ttu-id="862b3-119">tooenable hello RDMA-nätverksanslutning när du använder hello Resource Manager-distributionsmodellen, distribuera hello virtuella datorer i hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="862b3-119">tooenable hello RDMA network connection when you use hello Resource Manager deployment model, deploy hello VMs in hello same availability set.</span></span>
* <span data-ttu-id="862b3-120">**HPC Pack**: skapa ett Microsoft HPC Pack-kluster i Azure och lägga till RDMA-kompatibla compute-noder som kör ett stöds Linux distribution tooaccess hello RDMA-nätverk.</span><span class="sxs-lookup"><span data-stu-id="862b3-120">**HPC Pack**: Create a Microsoft HPC Pack cluster in Azure and add RDMA-capable compute nodes that run a supported Linux distribution tooaccess hello RDMA network.</span></span> <span data-ttu-id="862b3-121">Mer information finns i [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="862b3-121">For more information, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="sample-deployment-steps-in-hello-classic-model"></a><span data-ttu-id="862b3-122">Exempel distributionsstegen i hello klassiska modellen</span><span class="sxs-lookup"><span data-stu-id="862b3-122">Sample deployment steps in hello classic model</span></span>
<span data-ttu-id="862b3-123">hello följande steg visar hur toouse hello Azure CLI toodeploy SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM från hello Azure Marketplace, anpassa den och skapa en anpassad VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="862b3-123">hello following steps show how toouse hello Azure CLI toodeploy a SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM from hello Azure Marketplace, customize it, and create a custom VM image.</span></span> <span data-ttu-id="862b3-124">Du kan använda hello tooscript hello distribution av avbildningar i ett kluster med RDMA-kompatibla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="862b3-124">Then you can use hello image tooscript hello deployment of a cluster of RDMA-capable VMs.</span></span>

> [!TIP]
> <span data-ttu-id="862b3-125">Använd liknande steg toodeploy ett kluster med RDMA-kompatibla virtuella datorer baserat på CentOS-baserade HPC-avbildningar i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="862b3-125">Use similar steps toodeploy a cluster of RDMA-capable VMs based on CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="862b3-126">Vissa av stegen variera något som anges.</span><span class="sxs-lookup"><span data-stu-id="862b3-126">Some steps differ slightly, as noted.</span></span> 
>
>

### <a name="prerequisites"></a><span data-ttu-id="862b3-127">Krav</span><span class="sxs-lookup"><span data-stu-id="862b3-127">Prerequisites</span></span>
* <span data-ttu-id="862b3-128">**Klientdatorn**: du behöver en Mac, Linux eller Windows-klienten datorn toocommunicate med Azure.</span><span class="sxs-lookup"><span data-stu-id="862b3-128">**Client computer**: You need a Mac, Linux, or Windows client computer toocommunicate with Azure.</span></span> <span data-ttu-id="862b3-129">De här stegen förutsätter att du använder en Linux-klient.</span><span class="sxs-lookup"><span data-stu-id="862b3-129">These steps assume you are using a Linux client.</span></span>
* <span data-ttu-id="862b3-130">**Azure-prenumeration**: Om du inte har en prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="862b3-130">**Azure subscription**: If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span> <span data-ttu-id="862b3-131">Överväg att en prenumeration med användningsbaserad betalning eller andra köpalternativ för större kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-131">For larger clusters, consider a pay-as-you-go subscription or other purchase options.</span></span>
* <span data-ttu-id="862b3-132">**Tillgänglighet för VM-storlek**: hello följande instanser är RDMA-kompatibla: H16r, H16mr, A8 och A9.</span><span class="sxs-lookup"><span data-stu-id="862b3-132">**VM size availability**: hello following instance sizes are RDMA capable: H16r, H16mr, A8, and A9.</span></span> <span data-ttu-id="862b3-133">Kontrollera [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/) för tillgänglighet i Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="862b3-133">Check [Products available by region](https://azure.microsoft.com/regions/services/) for availability in Azure regions.</span></span>
* <span data-ttu-id="862b3-134">**Kärnor kvoten**: du kan behöva tooincrease hello kvoten för kärnor toodeploy ett kluster med beräkningsintensiva virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="862b3-134">**Cores quota**: You might need tooincrease hello quota of cores toodeploy a cluster of compute-intensive VMs.</span></span> <span data-ttu-id="862b3-135">Du måste till exempel minst 128 kärnor, om du vill toodeploy 8 A9 virtuella datorer som visas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="862b3-135">For example, you need at least 128 cores if you want toodeploy 8 A9 VMs as shown in this article.</span></span> <span data-ttu-id="862b3-136">Din prenumeration kan också begränsa hello antal kärnor som du kan distribuera i vissa VM storlek familjer, inklusive hello H-serien.</span><span class="sxs-lookup"><span data-stu-id="862b3-136">Your subscription might also limit hello number of cores you can deploy in certain VM size families, including hello H-series.</span></span> <span data-ttu-id="862b3-137">toorequest en kvot ökar, [öppna en supportbegäran online customer](../../../azure-supportability/how-to-create-azure-support-request.md) utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="862b3-137">toorequest a quota increase, [open an online customer support request](../../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
* <span data-ttu-id="862b3-138">**Azure CLI**: [installera](../../../cli-install-nodejs.md) hello Azure CLI och [ansluta tooyour Azure-prenumeration](../../../xplat-cli-connect.md) från hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="862b3-138">**Azure CLI**: [Install](../../../cli-install-nodejs.md) hello Azure CLI and [connect tooyour Azure subscription](../../../xplat-cli-connect.md) from hello client computer.</span></span>

### <a name="provision-an-sles-12-sp1-hpc-vm"></a><span data-ttu-id="862b3-139">Etablera en SLES 12 SP1 HPC-dator</span><span class="sxs-lookup"><span data-stu-id="862b3-139">Provision an SLES 12 SP1 HPC VM</span></span>
<span data-ttu-id="862b3-140">När du har loggat in tooAzure med hello Azure CLI kör `azure config list` tooconfirm som hello utdata visar Service Management-läge.</span><span class="sxs-lookup"><span data-stu-id="862b3-140">After signing in tooAzure with hello Azure CLI, run `azure config list` tooconfirm that hello output shows Service Management mode.</span></span> <span data-ttu-id="862b3-141">Om det inte ange hello läge genom att köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="862b3-141">If it does not, set hello mode by running this command:</span></span>

    azure config mode asm


<span data-ttu-id="862b3-142">Skriv följande toolist hello alla hello prenumerationer som du är behörig toouse:</span><span class="sxs-lookup"><span data-stu-id="862b3-142">Type hello following toolist all hello subscriptions you are authorized toouse:</span></span>

    azure account list

<span data-ttu-id="862b3-143">hello aktuella aktiv prenumeration identifieras med `Current` ställa in också`true`.</span><span class="sxs-lookup"><span data-stu-id="862b3-143">hello current active subscription is identified with `Current` set too`true`.</span></span> <span data-ttu-id="862b3-144">Om den här prenumerationen inte hello som du vill toouse toocreate hello klustret genom att ange hello lämpliga prenumerations-ID som hello aktiv prenumeration:</span><span class="sxs-lookup"><span data-stu-id="862b3-144">If this subscription isn't hello one you want toouse toocreate hello cluster, set hello appropriate subscription ID as hello active subscription:</span></span>

    azure account set <subscription-Id>

<span data-ttu-id="862b3-145">toosee hello offentligt tillgängliga SLES 12 SP1 HPC-avbildningar i Azure, köra ett kommando som hello efter, förutsatt att din miljö har stöd för shell **grep**:</span><span class="sxs-lookup"><span data-stu-id="862b3-145">toosee hello publicly available SLES 12 SP1 HPC images in Azure, run a command like hello following, assuming your shell environment supports **grep**:</span></span>

    azure vm image list | grep "suse.*hpc"

<span data-ttu-id="862b3-146">Etablera en RDMA-kompatibla virtuell dator med en SLES 12 SP1 HPC-avbildning genom att köra ett kommando som hello följande:</span><span class="sxs-lookup"><span data-stu-id="862b3-146">Provision an RDMA-capable VM with a SLES 12 SP1 HPC image by running a command like hello following:</span></span>

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

<span data-ttu-id="862b3-147">Där:</span><span class="sxs-lookup"><span data-stu-id="862b3-147">Where:</span></span>

* <span data-ttu-id="862b3-148">Hej storlek (A9 i det här exemplet) är en av RDMA-kompatibla hello VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="862b3-148">hello size (A9 in this example) is one of hello RDMA-capable VM sizes.</span></span>
* <span data-ttu-id="862b3-149">hello externa SSH-portnumret (22 i det här exemplet är hello SSH standard) är ett giltigt portnummer.</span><span class="sxs-lookup"><span data-stu-id="862b3-149">hello external SSH port number (22 in this example, which is hello SSH default) is any valid port number.</span></span> <span data-ttu-id="862b3-150">hello interna SSH-portnumret anges too22.</span><span class="sxs-lookup"><span data-stu-id="862b3-150">hello internal SSH port number is set too22.</span></span>
* <span data-ttu-id="862b3-151">En ny molntjänst skapas i hello Azure-region som anges av hello plats.</span><span class="sxs-lookup"><span data-stu-id="862b3-151">A new cloud service is created in hello Azure region specified by hello location.</span></span> <span data-ttu-id="862b3-152">Ange en plats som du väljer är tillgänglig i vilka hello VM.</span><span class="sxs-lookup"><span data-stu-id="862b3-152">Specify a location in which hello VM size you choose is available.</span></span>
* <span data-ttu-id="862b3-153">SUSE prioritet support (som debiteras ytterligare) hello SLES 12 SP1 avbildningsnamn för närvarande kan vara något av följande två alternativ:</span><span class="sxs-lookup"><span data-stu-id="862b3-153">For SUSE priority support (which incurs additional charges), hello SLES 12 SP1 image name currently can be one of these two options:</span></span> 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-hello-vm"></a><span data-ttu-id="862b3-154">Anpassa hello VM</span><span class="sxs-lookup"><span data-stu-id="862b3-154">Customize hello VM</span></span>
<span data-ttu-id="862b3-155">När hello VM har slutförts etablering SSH toohello VM med hjälp av hello extern IP-adress för Virtuella datorer (eller DNS-namn) och hello Externt portnummer du konfigurerade och anpassa den.</span><span class="sxs-lookup"><span data-stu-id="862b3-155">After hello VM finishes provisioning, SSH toohello VM by using hello VM's external IP address (or DNS name) and hello external port number you configured, and then customize it.</span></span> <span data-ttu-id="862b3-156">Anslutningsinformation finns [hur toolog på tooa virtuell dator som kör Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="862b3-156">For connection details, see [How toolog on tooa virtual machine running Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="862b3-157">Utför kommandon som hello användaren du konfigurerat på hello VM, såvida inte rotåtkomst är obligatoriska toocomplete ett steg.</span><span class="sxs-lookup"><span data-stu-id="862b3-157">Perform commands as hello user you configured on hello VM, unless root access is required toocomplete a step.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="862b3-158">Microsoft Azure tillhandahåller inte rotåtkomst tooLinux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="862b3-158">Microsoft Azure does not provide root access tooLinux VMs.</span></span> <span data-ttu-id="862b3-159">toogain administrativ åtkomst när du är ansluten som en användare toohello virtuell dator som kör kommandon med hjälp av `sudo`.</span><span class="sxs-lookup"><span data-stu-id="862b3-159">toogain administrative access when connected as a user toohello VM, run commands by using `sudo`.</span></span>
>
>

* <span data-ttu-id="862b3-160">**Uppdateringar**: Installera uppdateringar med hjälp av zypper.</span><span class="sxs-lookup"><span data-stu-id="862b3-160">**Updates**: Install updates by using zypper.</span></span> <span data-ttu-id="862b3-161">Du kan också tooinstall NFS verktyg.</span><span class="sxs-lookup"><span data-stu-id="862b3-161">You might also want tooinstall NFS utilities.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="862b3-162">I en SLES 12 SP1 HPC VM rekommenderar vi att du inte installerar kernel-uppdateringar, vilket kan orsaka problem med hello Linux RDMA drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="862b3-162">In a SLES 12 SP1 HPC VM, we recommend that you don't apply kernel updates, which can cause issues with hello Linux RDMA drivers.</span></span>
  >
  >
* <span data-ttu-id="862b3-163">**Intel MPI**: Slutför hello installationen av Intel MPI på hello SLES 12 SP1 HPC VM genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="862b3-163">**Intel MPI**: Complete hello installation of Intel MPI on hello SLES 12 SP1 HPC VM by running hello following command:</span></span>

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* <span data-ttu-id="862b3-164">**Låsa minne**: för MPI koder toolock hello minne tillgängligt för RDMA, lägga till eller ändra följande inställningar i hello /etc/security/limits.conf filen hello.</span><span class="sxs-lookup"><span data-stu-id="862b3-164">**Lock memory**: For MPI codes toolock hello memory available for RDMA, add or change hello following settings in hello /etc/security/limits.conf file.</span></span> <span data-ttu-id="862b3-165">Du måste roten åtkomst tooedit den här filen.</span><span class="sxs-lookup"><span data-stu-id="862b3-165">You need root access tooedit this file.</span></span>

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > <span data-ttu-id="862b3-166">I testsyfte kan ange du också memlock toounlimited.</span><span class="sxs-lookup"><span data-stu-id="862b3-166">For testing purposes, you can also set memlock toounlimited.</span></span> <span data-ttu-id="862b3-167">Till exempel: `<User or group name>    hard    memlock unlimited`.</span><span class="sxs-lookup"><span data-stu-id="862b3-167">For example: `<User or group name>    hard    memlock unlimited`.</span></span> <span data-ttu-id="862b3-168">Mer information finns i [kända bästa metoder för inställningen låst minnesstorleken](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span><span class="sxs-lookup"><span data-stu-id="862b3-168">For more information, see [Best known methods for setting locked memory size](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span></span>
  >
  >
* <span data-ttu-id="862b3-169">**SSH-nycklar för virtuella datorer SLES**: Generera SSH-nycklar tooestablish förtroende för ditt användarkonto bland hello compute-noder i hello SLES kluster när du kör MPI-jobb.</span><span class="sxs-lookup"><span data-stu-id="862b3-169">**SSH keys for SLES VMs**: Generate SSH keys tooestablish trust for your user account among hello compute nodes in hello SLES cluster when running MPI jobs.</span></span> <span data-ttu-id="862b3-170">Om du har distribuerat en CentOS-baserade HPC VM inte följer du detta steg.</span><span class="sxs-lookup"><span data-stu-id="862b3-170">If you deployed a CentOS-based HPC VM, don't follow this step.</span></span> <span data-ttu-id="862b3-171">Instruktioner senare i den här artikeln tooset passwordless SSH-förtroende mellan hello klusternoderna finns när du hello avbildning och distribuera hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-171">See instructions later in this article tooset up passwordless SSH trust among hello cluster nodes after you capture hello image and deploy hello cluster.</span></span>

    <span data-ttu-id="862b3-172">toocreate SSH-nycklar, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="862b3-172">toocreate SSH keys, run hello following command.</span></span> <span data-ttu-id="862b3-173">När du uppmanas att ange indata väljer **RETUR** toogenerate hello nycklar i hello standardplatsen utan att ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="862b3-173">When you are prompted for input, select **Enter** toogenerate hello keys in hello default location without setting a password.</span></span>

        ssh-keygen

    <span data-ttu-id="862b3-174">Tilläggsfilen hello offentliga nyckel toohello authorized_keys för kända offentliga nycklar.</span><span class="sxs-lookup"><span data-stu-id="862b3-174">Append hello public key toohello authorized_keys file for known public keys.</span></span>

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    <span data-ttu-id="862b3-175">I hello ~/.ssh directory, redigera eller skapa hello config-fil.</span><span class="sxs-lookup"><span data-stu-id="862b3-175">In hello ~/.ssh directory, edit or create hello config file.</span></span> <span data-ttu-id="862b3-176">Ange hello IP-adressintervall hello privat nätverk som du planerar toouse i Azure (10.32.0.0/16 i det här exemplet):</span><span class="sxs-lookup"><span data-stu-id="862b3-176">Provide hello IP address range of hello private network that you plan toouse in Azure (10.32.0.0/16 in this example):</span></span>

        host 10.32.0.*
        StrictHostKeyChecking no

    <span data-ttu-id="862b3-177">Du kan också visa hello privat nätverk IP-adressen för varje virtuell dator i klustret:</span><span class="sxs-lookup"><span data-stu-id="862b3-177">Alternatively, list hello private network IP address of each VM in your cluster as follows:</span></span>

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > <span data-ttu-id="862b3-178">Konfigurera `StrictHostKeyChecking no` kan skapa en säkerhetsrisk om en specifik IP-adress eller intervall har angetts.</span><span class="sxs-lookup"><span data-stu-id="862b3-178">Configuring `StrictHostKeyChecking no` can create a potential security risk when a specific IP address or range is not specified.</span></span>
  >
  >
* <span data-ttu-id="862b3-179">**Program**: installera alla program som du behöver eller utföra andra anpassningar innan hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="862b3-179">**Applications**: Install any applications you need or perform other customizations before you capture hello image.</span></span>

### <a name="capture-hello-image"></a><span data-ttu-id="862b3-180">Hello avbildning</span><span class="sxs-lookup"><span data-stu-id="862b3-180">Capture hello image</span></span>
<span data-ttu-id="862b3-181">toocapture hello avbildning, kör följande kommando på hello Linux VM hello först.</span><span class="sxs-lookup"><span data-stu-id="862b3-181">toocapture hello image, first run hello following command on hello Linux VM.</span></span> <span data-ttu-id="862b3-182">Det här kommandot deprovisions hello VM men underhåller användarkonton och SSH-nycklar som du skapat.</span><span class="sxs-lookup"><span data-stu-id="862b3-182">This command deprovisions hello VM but maintains user accounts and SSH keys that you set up.</span></span>

```
sudo waagent -deprovision
```

<span data-ttu-id="862b3-183">Kör följande Azure CLI-kommandon toocapture hello avbildningen hello från klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="862b3-183">From your client computer, run hello following Azure CLI commands toocapture hello image.</span></span> <span data-ttu-id="862b3-184">Mer information finns i [hur toocapture en klassisk virtuell Linux-dator som en bild](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="862b3-184">For more information, see [How toocapture a classic Linux virtual machine as an image](capture-image.md).</span></span>  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

<span data-ttu-id="862b3-185">När du har kört dessa kommandon hello VM avbildningen har gjorts för din användning och hello VM tas bort.</span><span class="sxs-lookup"><span data-stu-id="862b3-185">After you run these commands, hello VM image is captured for your use and hello VM is deleted.</span></span> <span data-ttu-id="862b3-186">Nu har du en anpassad avbildning redo toodeploy ett kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-186">Now you have your custom image ready toodeploy a cluster.</span></span>

### <a name="deploy-a-cluster-with-hello-image"></a><span data-ttu-id="862b3-187">Distribuera ett kluster med hello avbildning</span><span class="sxs-lookup"><span data-stu-id="862b3-187">Deploy a cluster with hello image</span></span>
<span data-ttu-id="862b3-188">Ändra hello följande Bash-skript med lämpliga värden för din miljö och köra den från din klientdator.</span><span class="sxs-lookup"><span data-stu-id="862b3-188">Modify hello following Bash script with appropriate values for your environment and run it from your client computer.</span></span> <span data-ttu-id="862b3-189">Eftersom Azure distribuerar hello VMs följd i hello klassiska distributionsmodellen, tar det några minuter toodeploy hello åtta A9 virtuella datorer i det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="862b3-189">Because Azure deploys hello VMs serially in hello classic deployment model, it takes a few minutes toodeploy hello eight A9 VMs suggested in this script.</span></span>

```
#!/bin/bash -x
# Script toocreate a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where hello VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All hello compute-intensive instances need toobe in hello same cloud service for Linux RDMA toowork across InfiniBand.
# Note: hello current maximum number of VMs in a cloud service is 50. If you need tooprovision more than 50 VMs in hello same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want tooturn off external ports and use only internal ports toocommunicate between compute nodes via port 22, don’t use this option. Since port numbers up too10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 toocluster18. Specify your captured image in <image-name>. Specify hello username and password you used when creating hello SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment tooprovision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a><span data-ttu-id="862b3-190">Överväganden för ett CentOS HPC-kluster</span><span class="sxs-lookup"><span data-stu-id="862b3-190">Considerations for a CentOS HPC cluster</span></span>
<span data-ttu-id="862b3-191">Tooset upp ett baserat på en av hello CentOS-baserade HPC-avbildningar i hello Azure Marketplace i stället för SLES 12 för HPC-kluster, så du hello allmänna steg i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="862b3-191">If you want tooset up a cluster based on one of hello CentOS-based HPC images in hello Azure Marketplace instead of SLES 12 for HPC, follow hello general steps in hello preceding section.</span></span> <span data-ttu-id="862b3-192">Observera följande skillnader när du etablerar och konfigurerar hello VM hello:</span><span class="sxs-lookup"><span data-stu-id="862b3-192">Note hello following differences when you provision and configure hello VM:</span></span>

- <span data-ttu-id="862b3-193">Intel MPI är redan installerad på en virtuell dator etablerats från en CentOS-baserade HPC-avbildning.</span><span class="sxs-lookup"><span data-stu-id="862b3-193">Intel MPI is already installed on a VM provisioned from a CentOS-based HPC image.</span></span>
- <span data-ttu-id="862b3-194">Lås minnesinställningarna har redan lagts till i hello VM /etc/security/limits.conf fil.</span><span class="sxs-lookup"><span data-stu-id="862b3-194">Lock memory settings are already added in hello VM's /etc/security/limits.conf file.</span></span>
- <span data-ttu-id="862b3-195">Generera inte SSH-nycklar på hello VM som du etablerar för avbildning.</span><span class="sxs-lookup"><span data-stu-id="862b3-195">Do not generate SSH keys on hello VM you provision for capture.</span></span> <span data-ttu-id="862b3-196">Vi rekommenderar i stället konfigurera användare för autentisering när du distribuerar hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-196">Instead, we recommend setting up user-based authentication after you deploy hello cluster.</span></span> <span data-ttu-id="862b3-197">Mer information finns i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="862b3-197">For more information, see hello following section.</span></span>  

### <a name="set-up-passwordless-ssh-trust-on-hello-cluster"></a><span data-ttu-id="862b3-198">Ställ in passwordless SSH-förtroende på hello klustret</span><span class="sxs-lookup"><span data-stu-id="862b3-198">Set up passwordless SSH trust on hello cluster</span></span>
<span data-ttu-id="862b3-199">Det finns två metoder för att upprätta förtroende mellan hello compute-noder på ett CentOS-baserade HPC-kluster: värd-baserad autentisering och användaren-baserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="862b3-199">On a CentOS-based HPC cluster, there are two methods for establishing trust between hello compute nodes: host-based authentication and user-based authentication.</span></span> <span data-ttu-id="862b3-200">Värd-baserad autentisering utanför hello omfånget för den här artikeln och vanligen måste göras via ett skript med filnamnstillägget under distributionen.</span><span class="sxs-lookup"><span data-stu-id="862b3-200">Host-based authentication is outside of hello scope of this article and generally must be done through an extension script during deployment.</span></span> <span data-ttu-id="862b3-201">Användare-baserad autentisering är praktiskt för att upprätta förtroende efter distributionen och kräver hello generation och delning av SSH-nycklar mellan hello compute-noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="862b3-201">User-based authentication is convenient for establishing trust after deployment and requires hello generation and sharing of SSH keys among hello compute nodes in hello cluster.</span></span> <span data-ttu-id="862b3-202">Den här metoden kallas passwordless SSH-inloggning och krävs när du kör MPI-jobb.</span><span class="sxs-lookup"><span data-stu-id="862b3-202">This method is commonly known as passwordless SSH login and is required when running MPI jobs.</span></span>

<span data-ttu-id="862b3-203">Ett exempelskript som bidrog från hello community är tillgängligt på [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable enkelt användarautentisering på ett CentOS-baserade HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-203">A sample script contributed from hello community is available on [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable easy user authentication on a CentOS-based HPC cluster.</span></span> <span data-ttu-id="862b3-204">Hämta och använda det här skriptet med hjälp av hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="862b3-204">Download and use this script by using hello following steps.</span></span> <span data-ttu-id="862b3-205">Du kan också ändra det här skriptet eller använda några andra metoden tooestablish passwordless SSH-autentisering mellan hello beräkning klusternoder.</span><span class="sxs-lookup"><span data-stu-id="862b3-205">You can also modify this script or use any other method tooestablish passwordless SSH authentication between hello cluster compute nodes.</span></span>

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

<span data-ttu-id="862b3-206">toorun hello skript, behöver du tooknow hello prefix för IP-adresser för undernät.</span><span class="sxs-lookup"><span data-stu-id="862b3-206">toorun hello script, you need tooknow hello prefix for your subnet IP addresses.</span></span> <span data-ttu-id="862b3-207">Hämta hello prefix genom att köra följande kommando på en av klusternoderna hello hello.</span><span class="sxs-lookup"><span data-stu-id="862b3-207">Get hello prefix by running hello following command on one of hello cluster nodes.</span></span> <span data-ttu-id="862b3-208">Din utdata ska se ut ungefär 10.1.3.5 och hello prefixet är hello 10.1.3 del.</span><span class="sxs-lookup"><span data-stu-id="862b3-208">Your output should look something like 10.1.3.5, and hello prefix is hello 10.1.3 portion.</span></span>

    ifconfig eth0 | grep -w inet | awk '{print $2}'

<span data-ttu-id="862b3-209">Nu köra hello skript med tre parametrar: hello vanliga användarnamn på hello compute-noder, hello gemensamt lösenord för användaren på hello datornoder och hello undernätsprefixets som returnerades från föregående hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="862b3-209">Now run hello script using three parameters: hello common user name on hello compute nodes, hello common password for that user on hello compute nodes, and hello subnet prefix that was returned from hello previous command.</span></span>

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

<span data-ttu-id="862b3-210">Det här skriptet hello följande:</span><span class="sxs-lookup"><span data-stu-id="862b3-210">This script does hello following:</span></span>

* <span data-ttu-id="862b3-211">Skapar en katalog på hello värdnoden med namnet .ssh, vilket krävs för passwordless inloggningen.</span><span class="sxs-lookup"><span data-stu-id="862b3-211">Creates a directory on hello host node named .ssh, which is required for passwordless login.</span></span>
* <span data-ttu-id="862b3-212">Skapar en konfigurationsfil i hello .ssh katalog som instruerar passwordless inloggning tooallow inloggning från alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="862b3-212">Creates a configuration file in hello .ssh directory that instructs passwordless login tooallow login from any node in hello cluster.</span></span>
* <span data-ttu-id="862b3-213">Skapa filer som innehåller hello nod-namn och IP-adresser i noden för alla hello-noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="862b3-213">Creates files containing hello node names and node IP addresses for all hello nodes in hello cluster.</span></span> <span data-ttu-id="862b3-214">De här filerna finns kvar efter hello skriptet körs för senare referens.</span><span class="sxs-lookup"><span data-stu-id="862b3-214">These files are left after hello script is run for later reference.</span></span>
* <span data-ttu-id="862b3-215">Skapar en privata och offentliga nyckel för varje nod i klustret (inklusive hello värdnoden) och skapar poster i hello authorized_keys fil.</span><span class="sxs-lookup"><span data-stu-id="862b3-215">Creates a private and public key pair for each cluster node (including hello host node) and creates entries in hello authorized_keys file.</span></span>

> [!WARNING]
> <span data-ttu-id="862b3-216">Kör skriptet kan du skapa en säkerhetsrisk.</span><span class="sxs-lookup"><span data-stu-id="862b3-216">Running this script can create a potential security risk.</span></span> <span data-ttu-id="862b3-217">Se till att hello offentlig nyckelinformation i ~/.ssh inte distribueras.</span><span class="sxs-lookup"><span data-stu-id="862b3-217">Ensure that hello public key information in ~/.ssh is not distributed.</span></span>
>
>

## <a name="configure-intel-mpi"></a><span data-ttu-id="862b3-218">Konfigurera Intel MPI</span><span class="sxs-lookup"><span data-stu-id="862b3-218">Configure Intel MPI</span></span>
<span data-ttu-id="862b3-219">toorun MPI program på Azure Linux RDMA, måste tooconfigure vissa miljö variabler specifika tooIntel MPI.</span><span class="sxs-lookup"><span data-stu-id="862b3-219">toorun MPI applications on Azure Linux RDMA, you need tooconfigure certain environment variables specific tooIntel MPI.</span></span> <span data-ttu-id="862b3-220">Här är ett exempel Bash-skript tooconfigure hello variabler som behövs toorun ett program.</span><span class="sxs-lookup"><span data-stu-id="862b3-220">Here is a sample Bash script tooconfigure hello variables needed toorun an application.</span></span> <span data-ttu-id="862b3-221">Ändra hello sökvägen toompivars.sh som behövs för installationen av Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="862b3-221">Change hello path toompivars.sh as needed for your installation of Intel MPI.</span></span>

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting hello variable tooshm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line toorun hello job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path toohello application exe> <arguments specific toohello application>

#end
```

<span data-ttu-id="862b3-222">hello hello värden filens format är som följer.</span><span class="sxs-lookup"><span data-stu-id="862b3-222">hello format of hello host file is as follows.</span></span> <span data-ttu-id="862b3-223">Lägg till en rad för varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="862b3-223">Add one line for each node in your cluster.</span></span> <span data-ttu-id="862b3-224">Ange den privata IP-adresser från hello virtuella nätverk som definierats tidigare, inte DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="862b3-224">Specify private IP addresses from hello virtual network defined earlier, not DNS names.</span></span> <span data-ttu-id="862b3-225">Till exempel innehåller två värdar med IP-adresser 10.32.0.1 och 10.32.0.2 hello filen hello följande:</span><span class="sxs-lookup"><span data-stu-id="862b3-225">For example, for two hosts with IP addresses 10.32.0.1 and 10.32.0.2, hello file contains hello following:</span></span>

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a><span data-ttu-id="862b3-226">Kör MPI på en grundläggande kluster med två noder</span><span class="sxs-lookup"><span data-stu-id="862b3-226">Run MPI on a basic two-node cluster</span></span>
<span data-ttu-id="862b3-227">Om du inte redan har gjort först ställa in hello miljö för Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="862b3-227">If you haven't already done so, first set up hello environment for Intel MPI.</span></span>

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a><span data-ttu-id="862b3-228">Kör MPI-kommando</span><span class="sxs-lookup"><span data-stu-id="862b3-228">Run an MPI command</span></span>
<span data-ttu-id="862b3-229">Kör MPI-kommando på en av hello compute-noder tooshow som MPI har installerats korrekt och kan kommunicera mellan minst två compute-noder.</span><span class="sxs-lookup"><span data-stu-id="862b3-229">Run an MPI command on one of hello compute nodes tooshow that MPI is installed properly and can communicate between at least two compute nodes.</span></span> <span data-ttu-id="862b3-230">hello följande **mpirun** körs hello **värdnamn** på två noder.</span><span class="sxs-lookup"><span data-stu-id="862b3-230">hello following **mpirun** command runs hello **hostname** command on two nodes.</span></span>

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
<span data-ttu-id="862b3-231">Dina utdata bör innehålla hello namnen på alla noder i hello som du har överfört som indata för `-hosts`.</span><span class="sxs-lookup"><span data-stu-id="862b3-231">Your output should list hello names of all hello nodes that you passed as input for `-hosts`.</span></span> <span data-ttu-id="862b3-232">Till exempel en **mpirun** kommando med två noder returnerar utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="862b3-232">For example, an **mpirun** command with two nodes returns output like hello following:</span></span>

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a><span data-ttu-id="862b3-233">Kör en MPI prestandamått</span><span class="sxs-lookup"><span data-stu-id="862b3-233">Run an MPI benchmark</span></span>
<span data-ttu-id="862b3-234">hello följande Intel MPI kommando kör en pingpong benchmark tooverify hello konfiguration och anslutningen toohello RDMA klusternätverk.</span><span class="sxs-lookup"><span data-stu-id="862b3-234">hello following Intel MPI command runs a pingpong benchmark tooverify hello cluster configuration and connection toohello RDMA network.</span></span>

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

<span data-ttu-id="862b3-235">Du bör se utdata som liknar följande hello på ett fungerande kluster med två noder.</span><span class="sxs-lookup"><span data-stu-id="862b3-235">On a working cluster with two nodes, you should see output like hello following.</span></span> <span data-ttu-id="862b3-236">Förvänta dig fördröjning vid eller under 3 mikrosekunder för meddelandestorleken in too512 byte i hello Azure RDMA nätverk.</span><span class="sxs-lookup"><span data-stu-id="862b3-236">On hello Azure RDMA network, expect latency at or below 3 microseconds for message sizes up too512 bytes.</span></span>

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# hello number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected toobe exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through hello flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks toorun:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a><span data-ttu-id="862b3-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="862b3-237">Next steps</span></span>
* <span data-ttu-id="862b3-238">Distribuera och köra din Linux MPI program på Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="862b3-238">Deploy and run your Linux MPI applications on your Linux cluster.</span></span>
* <span data-ttu-id="862b3-239">Se hello [Intel MPI biblioteket dokumentationen](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) vägledning om Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="862b3-239">See hello [Intel MPI Library documentation](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) for guidance on Intel MPI.</span></span>
* <span data-ttu-id="862b3-240">Försök en [quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate en Intel Lyster kluster med hjälp av en CentOS-baserade HPC-avbildning.</span><span class="sxs-lookup"><span data-stu-id="862b3-240">Try a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate an Intel Lustre cluster by using a CentOS-based HPC image.</span></span> <span data-ttu-id="862b3-241">Mer information finns i [distribuera Intel molnet Edition för Lyster på Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="862b3-241">For details, see [Deploying Intel Cloud Edition for Lustre on Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span></span>
