---
title: "aaaRun OpenFOAM med HPC Pack på virtuella Linux-datorer | Microsoft Docs"
description: "Distribuera ett kluster för Microsoft HPC Pack på Azure och kör en OpenFOAM jobb på flera Linux compute-noder i ett nätverk med RDMA."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: 1a51ffe6804abb5156f01d580c9b7a4a9649377a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a><span data-ttu-id="4fee2-103">Kör OpenFoam med Microsoft HPC Pack på ett Linux RDMA-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="4fee2-103">Run OpenFoam with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>
<span data-ttu-id="4fee2-104">Den här artikeln visar enkelriktade toorun OpenFoam i virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="4fee2-104">This article shows you one way toorun OpenFoam in Azure virtual machines.</span></span> <span data-ttu-id="4fee2-105">Här kan du distribuera ett Microsoft HPC Pack kluster med Linux compute-noder på Azure och kör en [OpenFoam](http://openfoam.com/) jobbet med Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="4fee2-105">Here, you deploy a Microsoft HPC Pack cluster with Linux compute nodes on Azure and run an [OpenFoam](http://openfoam.com/) job with Intel MPI.</span></span> <span data-ttu-id="4fee2-106">Du kan använda RDMA-kompatibla virtuella Azure-datorer för hello compute-noder så att hello datornoderna kommunicera över hello Azure RDMA nätverk.</span><span class="sxs-lookup"><span data-stu-id="4fee2-106">You can use RDMA-capable Azure VMs for hello compute nodes, so that hello compute nodes communicate over hello Azure RDMA network.</span></span> <span data-ttu-id="4fee2-107">Andra alternativ toorun OpenFoam i Azure är helt konfigurerade kommersiellt tillgängliga avbildningarna för hello Marketplace, till exempel Ubercloud's [OpenFoam 2.3 på CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), och genom att köra på [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span><span class="sxs-lookup"><span data-stu-id="4fee2-107">Other options toorun OpenFoam in Azure include fully configured commercial images available in hello Marketplace, such as UberCloud's [OpenFoam 2.3 on CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), and by running on [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/).</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="4fee2-108">OpenFOAM (för åtgärden Öppna fältet och manipulering) är en öppen källkod beräkningsprogram (CFD)-programpaket som används ofta i tekniker och vetenskap i både kommersiella och academic organisationer.</span><span class="sxs-lookup"><span data-stu-id="4fee2-108">OpenFOAM (for Open Field Operation and Manipulation) is an open-source computational fluid dynamics (CFD) software package that is used widely in engineering and science, in both commercial and academic organizations.</span></span> <span data-ttu-id="4fee2-109">Den innehåller verktyg för meshing, särskilt snappyHexMesh en parallelized mesher för komplexa CAD-geometrier och för före och efter bearbetning.</span><span class="sxs-lookup"><span data-stu-id="4fee2-109">It includes tools for meshing, notably snappyHexMesh, a parallelized mesher for complex CAD geometries, and for pre- and post-processing.</span></span> <span data-ttu-id="4fee2-110">Nästan alla processer som körs parallellt, aktivera användare tootake full nytta av datormaskinvara tillgång.</span><span class="sxs-lookup"><span data-stu-id="4fee2-110">Almost all processes run in parallel, enabling users tootake full advantage of computer hardware at their disposal.</span></span>  

<span data-ttu-id="4fee2-111">Microsoft HPC Pack ger funktioner toorun storskaliga HPC och parallella program, inklusive MPI program på kluster av Microsoft Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="4fee2-111">Microsoft HPC Pack provides features toorun large-scale HPC and parallel applications, including MPI applications, on clusters of Microsoft Azure virtual machines.</span></span> <span data-ttu-id="4fee2-112">HPC Pack stöder också körs Linux HPC-program på Linux-beräkningsnod virtuella datorer distribueras i ett kluster med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="4fee2-112">HPC Pack also supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="4fee2-113">Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) för en introduktion toousing Linux compute-noder med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="4fee2-113">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction toousing Linux compute nodes with HPC Pack.</span></span>

> [!NOTE]
> <span data-ttu-id="4fee2-114">Den här artikeln beskrivs hur toorun en Linux MPI arbetsbelastning med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="4fee2-114">This article illustrates how toorun a Linux MPI workload with HPC Pack.</span></span> <span data-ttu-id="4fee2-115">Det förutsätts att du vara bekant med Linux systemadministration och MPI arbetsbelastningar som körs på Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="4fee2-115">It assumes you have some familiarity with Linux system administration and with running MPI workloads on Linux clusters.</span></span> <span data-ttu-id="4fee2-116">Om du använder versioner av MPI och OpenFOAM skiljer sig från hello som visas i den här artikeln kan kanske du toomodify vissa installations- och konfigurationssteg.</span><span class="sxs-lookup"><span data-stu-id="4fee2-116">If you use versions of MPI and OpenFOAM different from hello ones shown in this article, you might have toomodify some installation and configuration steps.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="4fee2-117">Krav</span><span class="sxs-lookup"><span data-stu-id="4fee2-117">Prerequisites</span></span>
* <span data-ttu-id="4fee2-118">**HPC Pack kluster med RDMA-kompatibla Linux datornoder** – distribuera ett HPC Pack kluster med storlek A8 A9, H16r, eller H16rm Linux compute-noder med antingen en [Azure Resource Manager-mall](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) eller en [Azure PowerShell-skriptet](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="4fee2-118">**HPC Pack cluster with RDMA-capable Linux compute nodes** - Deploy an HPC Pack cluster with size A8, A9, H16r, or H16rm Linux compute nodes using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="4fee2-119">Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) hello krav och anvisningar för något av alternativen.</span><span class="sxs-lookup"><span data-stu-id="4fee2-119">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for hello prerequisites and steps for either option.</span></span> <span data-ttu-id="4fee2-120">Om du väljer hello distributionsalternativ för PowerShell-skript finns i hello exempelkonfigurationsfilen i hello exempelfiler hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4fee2-120">If you choose hello PowerShell script deployment option, see hello sample configuration file in hello sample files at hello end of this article.</span></span> <span data-ttu-id="4fee2-121">Använd den här konfigurationen toodeploy en Azure-baserade HPC Pack kluster som består av en storlek A8 Windows Server 2012 R2 huvudnod och 2 storlek A8 SUSE Linux Enterprise Server 12 compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-121">Use this configuration toodeploy an Azure-based HPC Pack cluster consisting of a size A8 Windows Server 2012 R2 head node and 2 size A8 SUSE Linux Enterprise Server 12 compute nodes.</span></span> <span data-ttu-id="4fee2-122">Ersätt lämpliga värden för namn på din prenumeration och tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4fee2-122">Substitute appropriate values for your subscription and service names.</span></span> 
  
  <span data-ttu-id="4fee2-123">**Ytterligare saker tooknow**</span><span class="sxs-lookup"><span data-stu-id="4fee2-123">**Additional things tooknow**</span></span>
  
  * <span data-ttu-id="4fee2-124">Linux RDMA nätverk krav i Azure, se [högpresterande compute VM-storlekar](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4fee2-124">For Linux RDMA networking prerequisites in Azure, see [High performance compute VM sizes](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  * <span data-ttu-id="4fee2-125">Om du använder hello distributionsalternativ för Powershell-skript kan du distribuera alla hello Linux compute-noder i ett moln service toouse hello RDMA-nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="4fee2-125">If you use hello Powershell script deployment option, deploy all hello Linux compute nodes within one cloud service toouse hello RDMA network connection.</span></span>
  * <span data-ttu-id="4fee2-126">Anslut med SSH tooperform ytterligare administrativa uppgifter när du har distribuerat hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-126">After deploying hello Linux nodes, connect by SSH tooperform any additional administrative tasks.</span></span> <span data-ttu-id="4fee2-127">Hitta hello SSH-anslutningsinformationen för varje Linux-VM i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4fee2-127">Find hello SSH connection details for each Linux VM in hello Azure portal.</span></span>  
* <span data-ttu-id="4fee2-128">**Intel MPI** -toorun OpenFOAM på SLES 12 HPC compute-noder i Azure måste du tooinstall hello Intel MPI biblioteket 5 runtime från hello [Intel.com plats](https://software.intel.com/en-us/intel-mpi-library/).</span><span class="sxs-lookup"><span data-stu-id="4fee2-128">**Intel MPI** - toorun OpenFOAM on SLES 12 HPC compute nodes in Azure, you need tooinstall hello Intel MPI Library 5 runtime from hello [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/).</span></span> <span data-ttu-id="4fee2-129">(Intel MPI 5 är förinstallerat på CentOS-baserade HPC bilder.)  Installera Intel MPI på dina Linux compute-noder i ett senare steg om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4fee2-129">(Intel MPI 5 is preinstalled on CentOS-based HPC images.)  In a later step, if necessary, install Intel MPI on your Linux compute nodes.</span></span> <span data-ttu-id="4fee2-130">tooprepare för det här steget när du har registrerat med Intel, länken hello i hello bekräftelse e-toohello relaterade webbsida.</span><span class="sxs-lookup"><span data-stu-id="4fee2-130">tooprepare for this step, after you register with Intel, follow hello link in hello confirmation email toohello related web page.</span></span> <span data-ttu-id="4fee2-131">Kopiera hello hämta hello .tgz filen för hello lämplig version av Intel MPI-länk.</span><span class="sxs-lookup"><span data-stu-id="4fee2-131">Then, copy hello download link for hello .tgz file for hello appropriate version of Intel MPI.</span></span> <span data-ttu-id="4fee2-132">Den här artikeln är baserad på Intel MPI version 5.0.3.048.</span><span class="sxs-lookup"><span data-stu-id="4fee2-132">This article is based on Intel MPI version 5.0.3.048.</span></span>
* <span data-ttu-id="4fee2-133">**OpenFOAM källa Pack** -ladda ned hello OpenFOAM källa Pack programvara för Linux från hello [OpenFOAM Foundation-webbplatsen](http://openfoam.org/download/2-3-1-source/).</span><span class="sxs-lookup"><span data-stu-id="4fee2-133">**OpenFOAM Source Pack** - Download hello OpenFOAM Source Pack software for Linux from hello [OpenFOAM Foundation site](http://openfoam.org/download/2-3-1-source/).</span></span> <span data-ttu-id="4fee2-134">Den här artikeln är baserad på källan Pack-version 2.3.1-baserad, hämtas som OpenFOAM 2.3.1.tgz.</span><span class="sxs-lookup"><span data-stu-id="4fee2-134">This article is based on Source Pack version 2.3.1, available for download as OpenFOAM-2.3.1.tgz.</span></span> <span data-ttu-id="4fee2-135">Följ instruktionerna för hello senare i den här artikeln toounpack och kompilera OpenFOAM på hello Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-135">Follow hello instructions later in this article toounpack and compile OpenFOAM on hello Linux compute nodes.</span></span>
* <span data-ttu-id="4fee2-136">**EnSight** (valfritt) – toosee hello resultaten av OpenFOAM simuleringen, hämta och installera hello [EnSight](https://www.ceisoftware.com/download/) visualisering och analys av programmet.</span><span class="sxs-lookup"><span data-stu-id="4fee2-136">**EnSight** (optional) - toosee hello results of your OpenFOAM simulation, download and install hello [EnSight](https://www.ceisoftware.com/download/) visualization and analysis program.</span></span> <span data-ttu-id="4fee2-137">Licensierings-och hämta är hello EnSight plats.</span><span class="sxs-lookup"><span data-stu-id="4fee2-137">Licensing and download information are at hello EnSight site.</span></span>

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="4fee2-138">Ställ in ömsesidigt förtroende mellan beräkningsnoder</span><span class="sxs-lookup"><span data-stu-id="4fee2-138">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="4fee2-139">Kör ett jobb mellan noder på flera Linux-noder kräver hello noder tootrust varandra (av **rsh** eller **ssh**).</span><span class="sxs-lookup"><span data-stu-id="4fee2-139">Running a cross-node job on multiple Linux nodes requires hello nodes tootrust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="4fee2-140">När du skapar hello HPC Pack kluster med hello Microsoft HPC Pack IaaS distributionsskriptet konfigurerar hello skript automatiskt permanenta ömsesidigt förtroende för hello administratörskontot som du anger.</span><span class="sxs-lookup"><span data-stu-id="4fee2-140">When you create hello HPC Pack cluster with hello Microsoft HPC Pack IaaS deployment script, hello script automatically sets up permanent mutual trust for hello administrator account you specify.</span></span> <span data-ttu-id="4fee2-141">Vanliga användare som du skapar i hello klustrets domän, har du tooset tillfälliga ömsesidigt förtroende mellan hello noder när ett jobb har allokerats toothem och förstöra hello relationen när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4fee2-141">For non-administrator users you create in hello cluster's domain, you have tooset up temporary mutual trust among hello nodes when a job is allocated toothem, and destroy hello relationship after hello job is complete.</span></span> <span data-ttu-id="4fee2-142">tooestablish förtroende för varje användare måste ange ett RSA nyckelpar toohello kluster med HPC Pack för hello förtroende.</span><span class="sxs-lookup"><span data-stu-id="4fee2-142">tooestablish trust for each user, provide an RSA key pair toohello cluster that HPC Pack uses for hello trust relationship.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="4fee2-143">Generera en RSA-nyckelpar</span><span class="sxs-lookup"><span data-stu-id="4fee2-143">Generate an RSA key pair</span></span>
<span data-ttu-id="4fee2-144">Det är enkelt toogenerate ett RSA-nyckelpar, som innehåller en offentlig nyckel och en privat nyckel, genom att köra hello Linux **ssh-keygen** kommando.</span><span class="sxs-lookup"><span data-stu-id="4fee2-144">It's easy toogenerate an RSA key pair, which contains a public key and a private key, by running hello Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="4fee2-145">Logga in tooa Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="4fee2-145">Log on tooa Linux computer.</span></span>
2. <span data-ttu-id="4fee2-146">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="4fee2-146">Run hello following command:</span></span>
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="4fee2-147">Tryck på **RETUR** toouse hello-standardinställningarna tills hello-kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4fee2-147">Press **Enter** toouse hello default settings until hello command is completed.</span></span> <span data-ttu-id="4fee2-148">Ange inte en lösenfras här. När du uppmanas att ange ett lösenord trycker du bara på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-148">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Generera en RSA-nyckelpar][keygen]
3. <span data-ttu-id="4fee2-150">Ändra katalogen toohello ~/.ssh katalog.</span><span class="sxs-lookup"><span data-stu-id="4fee2-150">Change directory toohello ~/.ssh directory.</span></span> <span data-ttu-id="4fee2-151">hello privata nyckeln lagras i id_rsa och hello offentliga nyckeln i id_rsa.pub.</span><span class="sxs-lookup"><span data-stu-id="4fee2-151">hello private key is stored in id_rsa and hello public key in id_rsa.pub.</span></span>
   
   ![Privata och offentliga nycklar][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a><span data-ttu-id="4fee2-153">Lägga till hello nyckelpar toohello HPC Pack kluster</span><span class="sxs-lookup"><span data-stu-id="4fee2-153">Add hello key pair toohello HPC Pack cluster</span></span>
1. <span data-ttu-id="4fee2-154">Gör en anslutning till Fjärrskrivbord anslutning tooyour huvudnod med ditt administratörskonto HPC Pack (hello-administratörskontot som du ställer in när du körde hello distributionsskriptet).</span><span class="sxs-lookup"><span data-stu-id="4fee2-154">Make a Remote Desktop connection tooyour head node with your HPC Pack administrator account (hello administrator account you set up when you ran hello deployment script).</span></span>
2. <span data-ttu-id="4fee2-155">Använd standard Windows Server procedurer toocreate ett domänanvändarkonto i hello klustrets Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="4fee2-155">Use standard Windows Server procedures toocreate a domain user account in hello cluster's Active Directory domain.</span></span> <span data-ttu-id="4fee2-156">Till exempel använda hello Active Directory-användare och datorer verktyget i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4fee2-156">For example, use hello Active Directory User and Computers tool on hello head node.</span></span> <span data-ttu-id="4fee2-157">hello exemplen i den här artikeln förutsätter att du skapar en domänanvändare med namnet hpclab\hpcuser.</span><span class="sxs-lookup"><span data-stu-id="4fee2-157">hello examples in this article assume you create a domain user named hpclab\hpcuser.</span></span>
3. <span data-ttu-id="4fee2-158">Skapa en fil med namnet C:\cred.xml och kopiera hello RSA viktiga data till den.</span><span class="sxs-lookup"><span data-stu-id="4fee2-158">Create a file named C:\cred.xml and copy hello RSA key data into it.</span></span> <span data-ttu-id="4fee2-159">En exempelfil cred.xml är hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4fee2-159">A sample cred.xml file is at hello end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. <span data-ttu-id="4fee2-160">Öppna en kommandotolk och ange följande kommando tooset hello autentiseringsuppgifter data för hello hpclab\hpcuser konto hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-160">Open a Command Prompt and enter hello following command tooset hello credentials data for hello hpclab\hpcuser account.</span></span> <span data-ttu-id="4fee2-161">Du använder hello **extendeddata** parametern toopass hello namnet C:\cred.xml fil du skapade hello viktiga data.</span><span class="sxs-lookup"><span data-stu-id="4fee2-161">You use hello **extendeddata** parameter toopass hello name of C:\cred.xml file you created for hello key data.</span></span>
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="4fee2-162">Det här kommandot har slutförts utan utdata.</span><span class="sxs-lookup"><span data-stu-id="4fee2-162">This command completes successfully without output.</span></span> <span data-ttu-id="4fee2-163">Lagra hello cred.xml filen på en säker plats när du har angett hello autentiseringsuppgifter för hello användarkonton som du behöver toorun jobb, eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="4fee2-163">After setting hello credentials for hello user accounts you need toorun jobs, store hello cred.xml file in a secure location, or delete it.</span></span>
5. <span data-ttu-id="4fee2-164">Om du har genererat hello RSA-nyckelpar på en Linux-noder kan du komma ihåg toodelete hello nycklar när du är klar med dem..</span><span class="sxs-lookup"><span data-stu-id="4fee2-164">If you generated hello RSA key pair on one of your Linux nodes, remember toodelete hello keys after you finish using them.</span></span> <span data-ttu-id="4fee2-165">Om HPC Pack hittar en befintlig id_rsa- eller id_rsa.pub fil, anger den inte ömsesidigt förtroende.</span><span class="sxs-lookup"><span data-stu-id="4fee2-165">If HPC Pack finds an existing id_rsa file or id_rsa.pub file, it does not set up mutual trust.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4fee2-166">Vi rekommenderar inte kör ett Linux-jobb som en Klusteradministratör i ett delat kluster, eftersom ett jobb som skickats av en administratör köras under rotkontot hello på hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-166">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under hello root account on hello Linux nodes.</span></span> <span data-ttu-id="4fee2-167">Dock körs ett jobb som skickats av en icke-administratörer under ett lokalt användarkonto för Linux med hello samma namn som hello jobbet användare.</span><span class="sxs-lookup"><span data-stu-id="4fee2-167">However, a job submitted by a non-administrator user runs under a local Linux user account with hello same name as hello job user.</span></span> <span data-ttu-id="4fee2-168">I det här fallet konfigurerar HPC Pack ömsesidigt förtroende för den här användaren Linux över hello noder allokeras toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="4fee2-168">In this case, HPC Pack sets up mutual trust for this Linux user across hello nodes allocated toohello job.</span></span> <span data-ttu-id="4fee2-169">Du kan ställa in hello Linux användare manuellt på hello Linux noder innan du kör jobbet hello eller HPC Pack skapar hello användaren automatiskt när hello jobbet har skickats.</span><span class="sxs-lookup"><span data-stu-id="4fee2-169">You can set up hello Linux user manually on hello Linux nodes before running hello job, or HPC Pack creates hello user automatically when hello job is submitted.</span></span> <span data-ttu-id="4fee2-170">Om HPC Pack skapar hello användare, HPC Pack tar bort den efter hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4fee2-170">If HPC Pack creates hello user, HPC Pack deletes it after hello job completes.</span></span> <span data-ttu-id="4fee2-171">tooreduce säkerhetshot, HPC Pack tar bort hello nycklar när jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4fee2-171">tooreduce security threats, HPC Pack removes hello keys after job completion.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="4fee2-172">Skapa en filresurs för Linux-noder</span><span class="sxs-lookup"><span data-stu-id="4fee2-172">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="4fee2-173">Nu ställa in en vanlig SMB-resurs på en mapp på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4fee2-173">Now set up a standard SMB share on a folder on hello head node.</span></span> <span data-ttu-id="4fee2-174">tooallow hello Linux noder tooaccess programfiler med en sökväg, montera hello delad mapp på hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-174">tooallow hello Linux nodes tooaccess application files with a common path, mount hello shared folder on hello Linux nodes.</span></span> <span data-ttu-id="4fee2-175">Om du vill kan använda du ett annat alternativ, till exempel filer för Azure-resurs - rekommenderas för många scenarier- eller en NFS-resurs för fildelning.</span><span class="sxs-lookup"><span data-stu-id="4fee2-175">If you want, you can use another file sharing option, such as an Azure Files share - recommended for many scenarios - or an NFS share.</span></span> <span data-ttu-id="4fee2-176">Se hello fildelning information och detaljerade anvisningar i [komma igång med Linux compute-noder i ett HPC Pack-kluster i Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="4fee2-176">See hello file sharing information and detailed steps in [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="4fee2-177">Skapa en mapp på hello huvudnod och dela den tooEveryone genom att ange behörighet för läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="4fee2-177">Create a folder on hello head node, and share it tooEveryone by setting Read/Write privileges.</span></span> <span data-ttu-id="4fee2-178">Till exempel dela C:\OpenFOAM i hello huvudnod som \\ \\SUSE12RDMA HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="4fee2-178">For example, share C:\OpenFOAM on hello head node as \\\\SUSE12RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="4fee2-179">Här, *SUSE12RDMA HN* är hello värdnamnet på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4fee2-179">Here, *SUSE12RDMA-HN* is hello host name of hello head node.</span></span>
2. <span data-ttu-id="4fee2-180">Öppna Windows PowerShell-fönstret och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="4fee2-180">Open a Windows PowerShell window and run hello following commands:</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

<span data-ttu-id="4fee2-181">hello första kommandot skapar en mapp med namnet /openfoam på alla noder i hello LinuxNodes grupp.</span><span class="sxs-lookup"><span data-stu-id="4fee2-181">hello first command creates a folder named /openfoam on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="4fee2-182">hello andra kommandot monterar hello delade mappen //SUSE12RDMA-HN/OpenFOAM på hello Linux noder med dir_mode och file_mode bits set too777.</span><span class="sxs-lookup"><span data-stu-id="4fee2-182">hello second command mounts hello shared folder //SUSE12RDMA-HN/OpenFOAM on hello Linux nodes with dir_mode and file_mode bits set too777.</span></span> <span data-ttu-id="4fee2-183">Hej *användarnamn* och *lösenord* hello kommandot ska vara hello autentiseringsuppgifterna för en användare i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4fee2-183">hello *username* and *password* in hello command should be hello credentials of a user on hello head node.</span></span>

> [!NOTE]
> <span data-ttu-id="4fee2-184">Hej ”\\`” symbol i andra hello-kommandot är en symbolen för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4fee2-184">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="4fee2-185">”\\`,” innebär hello ””, (kommatecken tecken) är en del av hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="4fee2-185">“\\`,” means hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

## <a name="install-mpi-and-openfoam"></a><span data-ttu-id="4fee2-186">Installera MPI och OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="4fee2-186">Install MPI and OpenFOAM</span></span>
<span data-ttu-id="4fee2-187">toorun OpenFOAM som ett MPI-jobb i hello RDMA nätverk måste toocompile OpenFOAM med hello Intel MPI-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="4fee2-187">toorun OpenFOAM as an MPI job on hello RDMA network, you need toocompile OpenFOAM with hello Intel MPI libraries.</span></span> 

<span data-ttu-id="4fee2-188">Kör först flera **clusrun** kommandon tooinstall Intel MPI bibliotek (om det inte redan är installerat) och OpenFOAM på Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-188">First, run several **clusrun** commands tooinstall Intel MPI libraries (if not already installed) and OpenFOAM on your Linux nodes.</span></span> <span data-ttu-id="4fee2-189">Använd hello huvudnod resursen konfigurerade tidigare tooshare hello installationsfiler hello Linux noderna.</span><span class="sxs-lookup"><span data-stu-id="4fee2-189">Use hello head node share configured previously tooshare hello installation files among hello Linux nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4fee2-190">Dessa installation och kompilerar stegen är exempel.</span><span class="sxs-lookup"><span data-stu-id="4fee2-190">These installation and compiling steps are examples.</span></span> <span data-ttu-id="4fee2-191">Du måste viss erfarenhet av Linux system administration tooensure att beroende kompilerare och bibliotek har installerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="4fee2-191">You need some knowledge of Linux system administration tooensure that dependent compilers and libraries are installed correctly.</span></span> <span data-ttu-id="4fee2-192">Du kanske måste toomodify vissa miljövariabler eller andra inställningar för din version av Intel MPI och OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="4fee2-192">You might need toomodify certain environment variables or other settings for your versions of Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="4fee2-193">Mer information finns i [Intel MPI-biblioteket för Linux-installationsguiden](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) och [OpenFOAM källa installationen](http://openfoam.org/download/2-3-1-source/) för din miljö.</span><span class="sxs-lookup"><span data-stu-id="4fee2-193">For details, see [Intel MPI Library for Linux Installation Guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) and [OpenFOAM Source Pack Installation](http://openfoam.org/download/2-3-1-source/) for your environment.</span></span>
> 
> 

### <a name="install-intel-mpi"></a><span data-ttu-id="4fee2-194">Installera Intel MPI</span><span class="sxs-lookup"><span data-stu-id="4fee2-194">Install Intel MPI</span></span>
<span data-ttu-id="4fee2-195">Spara hello hämtade installationspaketet för Intel MPI (l_mpi_p_5.0.3.048.tgz i det här exemplet) i C:\OpenFoam i hello huvudnod så att hello Linux noder kan komma åt den här filen från /openfoam.</span><span class="sxs-lookup"><span data-stu-id="4fee2-195">Save hello downloaded installation package for Intel MPI (l_mpi_p_5.0.3.048.tgz in this example) in C:\OpenFoam on hello head node so that hello Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="4fee2-196">Kör sedan **clusrun** tooinstall Intel MPI-biblioteket på alla noder av hello Linux.</span><span class="sxs-lookup"><span data-stu-id="4fee2-196">Then run **clusrun** tooinstall Intel MPI library on all hello Linux nodes.</span></span>

1. <span data-ttu-id="4fee2-197">hello följande kommandon kopiera hello installationspaketet och extraherar du det för/opt/intel på varje nod.</span><span class="sxs-lookup"><span data-stu-id="4fee2-197">hello following commands copy hello installation package and extract it too/opt/intel on each node.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. <span data-ttu-id="4fee2-198">tooinstall Intel MPI biblioteket använda tyst, en silent.cfg-fil.</span><span class="sxs-lookup"><span data-stu-id="4fee2-198">tooinstall Intel MPI Library silently, use a silent.cfg file.</span></span> <span data-ttu-id="4fee2-199">Du hittar ett exempel i hello exempelfiler hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4fee2-199">You can find an example in hello sample files at hello end of this article.</span></span> <span data-ttu-id="4fee2-200">Placera den här filen i hello delade mappen /openfoam.</span><span class="sxs-lookup"><span data-stu-id="4fee2-200">Place this file in hello shared folder /openfoam.</span></span> <span data-ttu-id="4fee2-201">Mer information om hello silent.cfg fil finns [Intel MPI-biblioteket för installationsguiden för Linux - tyst Installation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span><span class="sxs-lookup"><span data-stu-id="4fee2-201">For details about hello silent.cfg file, see [Intel MPI Library for Linux Installation Guide - Silent Installation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="4fee2-202">Kontrollera att du sparar filen som en textfil med Linux silent.cfg radbrytningar (endast LF, inte CR LF).</span><span class="sxs-lookup"><span data-stu-id="4fee2-202">Make sure that you save your silent.cfg file as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="4fee2-203">Det här steget säkerställer att den körs korrekt på hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-203">This step ensures that it runs properly on hello Linux nodes.</span></span>
   > 
   > 
3. <span data-ttu-id="4fee2-204">Installera Intel MPI biblioteket i tyst läge.</span><span class="sxs-lookup"><span data-stu-id="4fee2-204">Install Intel MPI Library in silent mode.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a><span data-ttu-id="4fee2-205">Konfigurera MPI</span><span class="sxs-lookup"><span data-stu-id="4fee2-205">Configure MPI</span></span>
<span data-ttu-id="4fee2-206">För att testa, bör du lägga till följande rader toohello /etc/security/limits.conf på varje nod för hello Linux hello:</span><span class="sxs-lookup"><span data-stu-id="4fee2-206">For testing, you should add hello following lines toohello /etc/security/limits.conf on each of hello Linux nodes:</span></span>

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


<span data-ttu-id="4fee2-207">Starta om hello Linux noder när du uppdaterar hello limits.conf fil.</span><span class="sxs-lookup"><span data-stu-id="4fee2-207">Restart hello Linux nodes after you update hello limits.conf file.</span></span> <span data-ttu-id="4fee2-208">Till exempel använder följande hello **clusrun** kommando:</span><span class="sxs-lookup"><span data-stu-id="4fee2-208">For example, use hello following **clusrun** command:</span></span>

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

<span data-ttu-id="4fee2-209">Se till att den delade mappen hello monterade som /openfoam efter omstart.</span><span class="sxs-lookup"><span data-stu-id="4fee2-209">After restarting, ensure that hello shared folder is mounted as /openfoam.</span></span>

### <a name="compile-and-install-openfoam"></a><span data-ttu-id="4fee2-210">Kompilera och installera OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="4fee2-210">Compile and install OpenFOAM</span></span>
<span data-ttu-id="4fee2-211">Spara hello hämtade installationspaketet för hello OpenFOAM källa Pack (OpenFOAM-2.3.1.tgz i det här exemplet) tooC:\OpenFoam i hello huvudnod så att hello Linux noder kan komma åt den här filen från /openfoam.</span><span class="sxs-lookup"><span data-stu-id="4fee2-211">Save hello downloaded installation package for hello OpenFOAM Source Pack (OpenFOAM-2.3.1.tgz in this example) tooC:\OpenFoam on hello head node so that hello Linux nodes can access this file from /openfoam.</span></span> <span data-ttu-id="4fee2-212">Kör sedan **clusrun** kommandon toocompile OpenFOAM på alla hello Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-212">Then run **clusrun** commands toocompile OpenFOAM on all hello Linux nodes.</span></span>

1. <span data-ttu-id="4fee2-213">Skapa en mapp /opt/OpenFOAM på varje Linux-nod, kopiera hello paketet toothis källmappen, och extrahera det det.</span><span class="sxs-lookup"><span data-stu-id="4fee2-213">Create a folder /opt/OpenFOAM on each Linux node, copy hello source package toothis folder, and extract it there.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. <span data-ttu-id="4fee2-214">toocompile OpenFOAM med hello Intel MPI biblioteket först ställa in miljövariabler för både Intel MPI och OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="4fee2-214">toocompile OpenFOAM with hello Intel MPI Library, first set up some environment variables for both Intel MPI and OpenFOAM.</span></span> <span data-ttu-id="4fee2-215">Använd ett bash-skript som heter settings.sh tooset hello variabler.</span><span class="sxs-lookup"><span data-stu-id="4fee2-215">Use a bash script called settings.sh tooset hello variables.</span></span> <span data-ttu-id="4fee2-216">Du hittar ett exempel i hello exempelfiler hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4fee2-216">You can find an example in hello sample files at hello end of this article.</span></span> <span data-ttu-id="4fee2-217">Placera den här filen (som sparas med radbrytningar för Linux) i hello delade mappen /openfoam.</span><span class="sxs-lookup"><span data-stu-id="4fee2-217">Place this file (saved with Linux line endings) in hello shared folder /openfoam.</span></span> <span data-ttu-id="4fee2-218">Den här filen innehåller inställningar för hello MPI och OpenFOAM körningar som du använder senare toorun ett OpenFOAM-jobb.</span><span class="sxs-lookup"><span data-stu-id="4fee2-218">This file also contains settings for hello MPI and OpenFOAM runtimes that you use later toorun an OpenFOAM job.</span></span>
3. <span data-ttu-id="4fee2-219">Installera beroende paket som behövs toocompile OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="4fee2-219">Install dependent packages needed toocompile OpenFOAM.</span></span> <span data-ttu-id="4fee2-220">Beroende på Linux-distribution måste du kanske först tooadd en databas.</span><span class="sxs-lookup"><span data-stu-id="4fee2-220">Depending on your Linux distribution, you might first need tooadd a repository.</span></span> <span data-ttu-id="4fee2-221">Kör **clusrun** liknande toohello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4fee2-221">Run **clusrun** commands similar toohello following:</span></span>
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    <span data-ttu-id="4fee2-222">Om det behövs kommandon SSH tooeach Linux nod toorun hello tooconfirm som de fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="4fee2-222">If necessary, SSH tooeach Linux node toorun hello commands tooconfirm that they run properly.</span></span>
4. <span data-ttu-id="4fee2-223">Kör följande kommando toocompile OpenFOAM hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-223">Run hello following command toocompile OpenFOAM.</span></span> <span data-ttu-id="4fee2-224">hello kompileringsprocessen tar några tid toocomplete och genererar en stor mängd information toostandard utdata, så Använd hello **/ överlagrat** alternativet toodisplay hello utdata överlagrat.</span><span class="sxs-lookup"><span data-stu-id="4fee2-224">hello compilation process takes some time toocomplete and generates a large amount of log information toostandard output, so use hello **/interleaved** option toodisplay hello output interleaved.</span></span>
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > <span data-ttu-id="4fee2-225">Hej ”\\`” symbol i hello-kommandot är en symbolen för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4fee2-225">hello “\\`” symbol in hello command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="4fee2-226">”\\`&” innebär hello ”&” är en del av hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="4fee2-226">“\\`&” means hello “&” is a part of hello command.</span></span>
   > 
   > 

## <a name="prepare-toorun-an-openfoam-job"></a><span data-ttu-id="4fee2-227">Förbereda toorun ett OpenFOAM-jobb</span><span class="sxs-lookup"><span data-stu-id="4fee2-227">Prepare toorun an OpenFOAM job</span></span>
<span data-ttu-id="4fee2-228">Nu get redo toorun ett MPI-jobb kallas sloshingTank3D som är en av hello OpenFoam prov på två Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-228">Now get ready toorun an MPI job called sloshingTank3D, which is one of hello OpenFoam samples, on two Linux nodes.</span></span> 

### <a name="set-up-hello-runtime-environment"></a><span data-ttu-id="4fee2-229">Ställ in hello körningsmiljön</span><span class="sxs-lookup"><span data-stu-id="4fee2-229">Set up hello runtime environment</span></span>
<span data-ttu-id="4fee2-230">tooset in hello runtime miljöer för MPI och OpenFOAM hello Linux-noder som kör följande kommando i Windows PowerShell-fönstret på hello huvudnod hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-230">tooset up hello runtime environments for MPI and OpenFOAM on hello Linux nodes, run hello following command in a Windows PowerShell window on hello head node.</span></span> <span data-ttu-id="4fee2-231">(Det här kommandot är giltig för SUSE Linux endast.)</span><span class="sxs-lookup"><span data-stu-id="4fee2-231">(This command is valid for SUSE Linux only.)</span></span>

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a><span data-ttu-id="4fee2-232">Förbereda exempeldata</span><span class="sxs-lookup"><span data-stu-id="4fee2-232">Prepare sample data</span></span>
<span data-ttu-id="4fee2-233">Använd hello huvudnod resursen du tidigare konfigurerat tooshare filer mellan hello Linux noder (monterade som /openfoam).</span><span class="sxs-lookup"><span data-stu-id="4fee2-233">Use hello head node share you configured previously tooshare files among hello Linux nodes (mounted as /openfoam).</span></span>

1. <span data-ttu-id="4fee2-234">SSH tooone av din Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-234">SSH tooone of your Linux compute nodes.</span></span>
2. <span data-ttu-id="4fee2-235">Kör hello efter kommandot tooset in hello OpenFOAM körningsmiljö, om du inte redan har gjort detta.</span><span class="sxs-lookup"><span data-stu-id="4fee2-235">Run hello following command tooset up hello OpenFOAM runtime environment, if you haven’t already done this.</span></span>
   
   ```
   $ source /openfoam/settings.sh
   ```
3. <span data-ttu-id="4fee2-236">Kopiera hello sloshingTank3D toohello delade mapp och navigera tooit.</span><span class="sxs-lookup"><span data-stu-id="4fee2-236">Copy hello sloshingTank3D sample toohello shared folder and navigate tooit.</span></span>
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. <span data-ttu-id="4fee2-237">När du använder hello-standardparametrar för det här exemplet, kan det ta flera minuter toorun så du kanske vill toomodify vissa parametrar toomake det snabbare.</span><span class="sxs-lookup"><span data-stu-id="4fee2-237">When you use hello default parameters of this sample, it can take tens of minutes toorun, so you might want toomodify some parameters toomake it run faster.</span></span> <span data-ttu-id="4fee2-238">Ett enkelt alternativ är toomodify hello tid step variabler deltaT och writeInterval i hello system/controlDict-filen.</span><span class="sxs-lookup"><span data-stu-id="4fee2-238">One simple choice is toomodify hello time step variables deltaT and writeInterval in hello system/controlDict file.</span></span> <span data-ttu-id="4fee2-239">Den här filen lagras alla indata som rör toohello kontroll över tid och läsning och skrivning av data för lösningen.</span><span class="sxs-lookup"><span data-stu-id="4fee2-239">This file stores all input data relating toohello control of time and reading and writing solution data.</span></span> <span data-ttu-id="4fee2-240">Du kan till exempel ändra hello värde av deltaT från 0,05 too0.5 och writeInterval från 0,05 too0.5 hello värde.</span><span class="sxs-lookup"><span data-stu-id="4fee2-240">For example, you could change hello value of deltaT from 0.05 too0.5 and hello value of writeInterval from 0.05 too0.5.</span></span>
   
   ![Ändra steg variabler][step_variables]
5. <span data-ttu-id="4fee2-242">Ange önskade värden för hello variabler i hello system/decomposeParDict-filen.</span><span class="sxs-lookup"><span data-stu-id="4fee2-242">Specify desired values for hello variables in hello system/decomposeParDict file.</span></span> <span data-ttu-id="4fee2-243">Det här exemplet använder två Linux noder varje med 8 kärnor, så ange numberOfSubdomains too16 och n för hierarchicalCoeffs too(1 1 16), vilket betyder att köra OpenFOAM parallellt med 16 processer.</span><span class="sxs-lookup"><span data-stu-id="4fee2-243">This example uses two Linux nodes each with 8 cores, so set numberOfSubdomains too16 and n of hierarchicalCoeffs too(1 1 16), which means run OpenFOAM in parallel with 16 processes.</span></span> <span data-ttu-id="4fee2-244">Mer information finns i [OpenFOAM användarhandboken: 3,4 program som körs parallellt](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span><span class="sxs-lookup"><span data-stu-id="4fee2-244">For more information, see [OpenFOAM User Guide: 3.4 Running applications in parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).</span></span>
   
   ![Dela upp processer][decompose]
6. <span data-ttu-id="4fee2-246">Kör följande kommandon från hello sloshingTank3D directory tooprepare hello exempeldata hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-246">Run hello following commands from hello sloshingTank3D directory tooprepare hello sample data.</span></span>
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. <span data-ttu-id="4fee2-247">Du bör se hello exempeldatafiler kopieras till C:\OpenFoam\sloshingTank3D i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4fee2-247">On hello head node, you should see hello sample data files are copied into C:\OpenFoam\sloshingTank3D.</span></span> <span data-ttu-id="4fee2-248">(C:\OpenFoam är hello delad mapp på hello huvudnod.)</span><span class="sxs-lookup"><span data-stu-id="4fee2-248">(C:\OpenFoam is hello shared folder on hello head node.)</span></span>
   
   ![Datafiler på hello huvudnod][data_files]

### <a name="host-file-for-mpirun"></a><span data-ttu-id="4fee2-250">Värdfilen för mpirun</span><span class="sxs-lookup"><span data-stu-id="4fee2-250">Host file for mpirun</span></span>
<span data-ttu-id="4fee2-251">I det här steget skapar du en värd fil (en lista över compute-noder) vilka hello **mpirun** kommando använder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-251">In this step, you create a host file (a list of compute nodes) which hello **mpirun** command uses.</span></span>

1. <span data-ttu-id="4fee2-252">På en av hello Linux noder, skapa en fil med namnet hostfile under /openfoam, så den här filen kan nås på /openfoam/hostfile på alla noder i Linux.</span><span class="sxs-lookup"><span data-stu-id="4fee2-252">On one of hello Linux nodes, create a file named hostfile under /openfoam, so this file can be reached at /openfoam/hostfile on all Linux nodes.</span></span>
2. <span data-ttu-id="4fee2-253">Skriv din Linux nod-namn i den här filen.</span><span class="sxs-lookup"><span data-stu-id="4fee2-253">Write your Linux node names into this file.</span></span> <span data-ttu-id="4fee2-254">I det här exemplet innehåller hello filen hello följande namn:</span><span class="sxs-lookup"><span data-stu-id="4fee2-254">In this example, hello file contains hello following names:</span></span>
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > <span data-ttu-id="4fee2-255">Du kan också skapa den här filen på C:\OpenFoam\hostfile i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="4fee2-255">You can also create this file at C:\OpenFoam\hostfile on hello head node.</span></span> <span data-ttu-id="4fee2-256">Om du väljer det här alternativet, spara den som en textfil med Linux radbrytningar (endast LF, inte CR LF).</span><span class="sxs-lookup"><span data-stu-id="4fee2-256">If you choose this option, save it as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="4fee2-257">Detta säkerställer att den körs korrekt på hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-257">This ensures that it runs properly on hello Linux nodes.</span></span>
   > 
   > 
   
   <span data-ttu-id="4fee2-258">**Omslutning för Bash-skript**</span><span class="sxs-lookup"><span data-stu-id="4fee2-258">**Bash script wrapper**</span></span>
   
   <span data-ttu-id="4fee2-259">Om du har många Linux noder och du vill att dina jobb toorun på bara några av dem, är det inte en bra idé toouse en fast värd-fil, eftersom du inte vet vilka noder allokeras tooyour jobb.</span><span class="sxs-lookup"><span data-stu-id="4fee2-259">If you have many Linux nodes and you want your job toorun on only some of them, it’s not a good idea toouse a fixed host file, because you don’t know which nodes will be allocated tooyour job.</span></span> <span data-ttu-id="4fee2-260">I det här fallet skriva ett bash-skript Omslutning för **mpirun** toocreate hello värdfilen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4fee2-260">In this case, write a bash script wrapper for **mpirun** toocreate hello host file automatically.</span></span> <span data-ttu-id="4fee2-261">Du kan hitta ett exempel bash-skript wrapper kallas hpcimpirun.sh hello slutet av den här artikeln och spara den som /openfoam/hpcimpirun.sh. Det här exempelskriptet hello följande:</span><span class="sxs-lookup"><span data-stu-id="4fee2-261">You can find an example bash script wrapper called hpcimpirun.sh at hello end of this article, and save it as /openfoam/hpcimpirun.sh. This example script does hello following:</span></span>
   
   1. <span data-ttu-id="4fee2-262">Ställer in hello miljövariabler för **mpirun**, och vissa tillägg kommandot parametrar toorun hello MPI jobb via hello RDMA nätverk.</span><span class="sxs-lookup"><span data-stu-id="4fee2-262">Sets up hello environment variables for **mpirun**, and some addition command parameters toorun hello MPI job through hello RDMA network.</span></span> <span data-ttu-id="4fee2-263">I det här fallet anger hello följande variabler:</span><span class="sxs-lookup"><span data-stu-id="4fee2-263">In this case, it sets hello following variables:</span></span>
      
      * <span data-ttu-id="4fee2-264">I_MPI_FABRICS = shm:dapl</span><span class="sxs-lookup"><span data-stu-id="4fee2-264">I_MPI_FABRICS=shm:dapl</span></span>
      * <span data-ttu-id="4fee2-265">I_MPI_DAPL_PROVIDER = en-v2-ib0</span><span class="sxs-lookup"><span data-stu-id="4fee2-265">I_MPI_DAPL_PROVIDER=ofa-v2-ib0</span></span>
      * <span data-ttu-id="4fee2-266">I_MPI_DYNAMIC_CONNECTION = 0</span><span class="sxs-lookup"><span data-stu-id="4fee2-266">I_MPI_DYNAMIC_CONNECTION=0</span></span>
   2. <span data-ttu-id="4fee2-267">Skapar en värd-fil enligt toohello miljö variabeln $CCP_NODES_CORES som anges av hello HPC-huvudnoden när hello jobbet är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="4fee2-267">Creates a host file according toohello environment variable $CCP_NODES_CORES, which is set by hello HPC head node when hello job is activated.</span></span>
      
      <span data-ttu-id="4fee2-268">hello format för $CCP_NODES_CORES följer detta mönster:</span><span class="sxs-lookup"><span data-stu-id="4fee2-268">hello format of $CCP_NODES_CORES follows this pattern:</span></span>
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      <span data-ttu-id="4fee2-269">där</span><span class="sxs-lookup"><span data-stu-id="4fee2-269">where</span></span>
      
      * <span data-ttu-id="4fee2-270">`<Number of nodes>`-hello antalet noder allokeras toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="4fee2-270">`<Number of nodes>` - hello number of nodes allocated toothis job.</span></span>  
      * <span data-ttu-id="4fee2-271">`<Name of node_n_...>`-hello namnet på varje nod allokerade toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="4fee2-271">`<Name of node_n_...>` - hello name of each node allocated toothis job.</span></span>
      * <span data-ttu-id="4fee2-272">`<Cores of node_n_...>`-hello antalet kärnor på hello nod allokerade toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="4fee2-272">`<Cores of node_n_...>` - hello number of cores on hello node allocated toothis job.</span></span>
      
      <span data-ttu-id="4fee2-273">Till exempel om hello jobbet måste två noder toorun, liknar $CCP_NODES_CORES</span><span class="sxs-lookup"><span data-stu-id="4fee2-273">For example, if hello job needs two nodes toorun, $CCP_NODES_CORES is similar to</span></span>
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. <span data-ttu-id="4fee2-274">Anrop hello **mpirun** kommando och lägger till två parametrar toohello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="4fee2-274">Calls hello **mpirun** command and appends two parameters toohello command line.</span></span>
      
      * <span data-ttu-id="4fee2-275">`--hostfile <hostfilepath>: <hostfilepath>`-hello sökvägen hello värden hello skript skapar</span><span class="sxs-lookup"><span data-stu-id="4fee2-275">`--hostfile <hostfilepath>: <hostfilepath>` - hello path of hello host file hello script creates</span></span>
      * <span data-ttu-id="4fee2-276">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-en miljövariabel som angetts av hello HPC Pack huvudnod, som lagrar hello antal totala kärnor allokerade toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="4fee2-276">`-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` - an environment variable set by hello HPC Pack head node, which stores hello number of total cores allocated toothis job.</span></span> <span data-ttu-id="4fee2-277">I det här fallet den anger hello antal processer för **mpirun**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-277">In this case, it specifies hello number of processes for **mpirun**.</span></span>

## <a name="submit-an-openfoam-job"></a><span data-ttu-id="4fee2-278">Skicka ett OpenFOAM-jobb</span><span class="sxs-lookup"><span data-stu-id="4fee2-278">Submit an OpenFOAM job</span></span>
<span data-ttu-id="4fee2-279">Nu kan du skicka ett jobb i HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="4fee2-279">Now you can submit a job in HPC Cluster Manager.</span></span> <span data-ttu-id="4fee2-280">Du måste toopass hello skriptet hpcimpirun.sh i hello kommandorader för några av hello jobbuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4fee2-280">You need toopass hello script hpcimpirun.sh in hello command lines for some of hello job tasks.</span></span>

1. <span data-ttu-id="4fee2-281">Anslut tooyour klustrets huvudnod och starta HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="4fee2-281">Connect tooyour cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="4fee2-282">**I resurshantering**, se till att hello Linux datornoderna hello **Online** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4fee2-282">**In Resource Management**, ensure that hello Linux compute nodes are in hello **Online** state.</span></span> <span data-ttu-id="4fee2-283">Om du inte markerar du dem och klickar på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-283">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="4fee2-284">I **jobbhantering**, klickar du på **nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-284">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="4fee2-285">Ange ett namn för jobbet som *sloshingTank3D*.</span><span class="sxs-lookup"><span data-stu-id="4fee2-285">Enter a name for job such as *sloshingTank3D*.</span></span>
   
   ![Jobbinformation][job_details]
5. <span data-ttu-id="4fee2-287">I **jobbet resurser**, Välj hello typ av resurs som ”nod” och ange hello minsta too2.</span><span class="sxs-lookup"><span data-stu-id="4fee2-287">In **Job resources**, choose hello type of resource as “Node” and set hello Minimum too2.</span></span> <span data-ttu-id="4fee2-288">Den här konfigurationen kör hello jobb på två noder i Linux, var och en har åtta kärnor i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4fee2-288">This configuration runs hello job on two Linux nodes, each of which has eight cores in this example.</span></span>
   
   ![Jobbet resurser][job_resources]
6. <span data-ttu-id="4fee2-290">Klicka på **redigera uppgifter** i hello vänstra navigeringsfönstret och klicka sedan på **Lägg till** tooadd en toohello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="4fee2-290">Click **Edit Tasks** in hello left navigation, and then click **Add** tooadd a task toohello job.</span></span> <span data-ttu-id="4fee2-291">Lägg till fyra uppgifter toohello jobbet med hello följande kommandorader och inställningar.</span><span class="sxs-lookup"><span data-stu-id="4fee2-291">Add four tasks toohello job with hello following command lines and settings.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4fee2-292">Kör `source /openfoam/settings.sh` ställer in hello OpenFOAM och MPI runtime miljöer, så var och en av följande uppgifter hello anropar den innan hello OpenFOAM kommando.</span><span class="sxs-lookup"><span data-stu-id="4fee2-292">Running `source /openfoam/settings.sh` sets up hello OpenFOAM and MPI runtime environments, so each of hello following tasks calls it before hello OpenFOAM command.</span></span>
   > 
   > 
   
   * <span data-ttu-id="4fee2-293">**Uppgift 1**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-293">**Task 1**.</span></span> <span data-ttu-id="4fee2-294">Kör **decomposePar** toogenerate filer för att köra **interDyMFoam** parallellt.</span><span class="sxs-lookup"><span data-stu-id="4fee2-294">Run **decomposePar** toogenerate data files for running **interDyMFoam** in parallel.</span></span>
     
     * <span data-ttu-id="4fee2-295">Tilldela en nod toohello aktivitet</span><span class="sxs-lookup"><span data-stu-id="4fee2-295">Assign one node toohello task</span></span>
     * <span data-ttu-id="4fee2-296">**Kommandorad** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="4fee2-296">**Command line** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="4fee2-297">**Arbetskatalogen** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="4fee2-297">**Working directory** - /openfoam/sloshingTank3D</span></span>
     
     <span data-ttu-id="4fee2-298">Se följande figur hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-298">See hello following figure.</span></span> <span data-ttu-id="4fee2-299">Du konfigurerar hello återstående uppgifterna på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="4fee2-299">You configure hello remaining tasks similarly.</span></span>
     
     ![Uppgift 1 information][task_details1]
   * <span data-ttu-id="4fee2-301">**Uppgift 2**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-301">**Task 2**.</span></span> <span data-ttu-id="4fee2-302">Kör **interDyMFoam** i parallella toocompute hello exemplet.</span><span class="sxs-lookup"><span data-stu-id="4fee2-302">Run **interDyMFoam** in parallel toocompute hello sample.</span></span>
     
     * <span data-ttu-id="4fee2-303">Tilldela två noder toohello uppgift</span><span class="sxs-lookup"><span data-stu-id="4fee2-303">Assign two nodes toohello task</span></span>
     * <span data-ttu-id="4fee2-304">**Kommandorad** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="4fee2-304">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="4fee2-305">**Arbetskatalogen** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="4fee2-305">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="4fee2-306">**Uppgift 3**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-306">**Task 3**.</span></span> <span data-ttu-id="4fee2-307">Kör **reconstructPar** toomerge hello anger tid kataloger från varje processor_N_ katalog till en enda uppsättning.</span><span class="sxs-lookup"><span data-stu-id="4fee2-307">Run **reconstructPar** toomerge hello sets of time directories from each processor_N_ directory into a single set.</span></span>
     
     * <span data-ttu-id="4fee2-308">Tilldela en nod toohello aktivitet</span><span class="sxs-lookup"><span data-stu-id="4fee2-308">Assign one node toohello task</span></span>
     * <span data-ttu-id="4fee2-309">**Kommandorad** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="4fee2-309">**Command line** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="4fee2-310">**Arbetskatalogen** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="4fee2-310">**Working directory** - /openfoam/sloshingTank3D</span></span>
   * <span data-ttu-id="4fee2-311">**Uppgift 4**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-311">**Task 4**.</span></span> <span data-ttu-id="4fee2-312">Kör **foamToEnsight** i parallella tooconvert hello OpenFOAM resultatet filerna till EnSight formatera och placera hello EnSight filer i en katalog med namnet Ensight i hello case directory.</span><span class="sxs-lookup"><span data-stu-id="4fee2-312">Run **foamToEnsight** in parallel tooconvert hello OpenFOAM result files into EnSight format and place hello EnSight files in a directory named Ensight in hello case directory.</span></span>
     
     * <span data-ttu-id="4fee2-313">Tilldela två noder toohello uppgift</span><span class="sxs-lookup"><span data-stu-id="4fee2-313">Assign two nodes toohello task</span></span>
     * <span data-ttu-id="4fee2-314">**Kommandorad** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span><span class="sxs-lookup"><span data-stu-id="4fee2-314">**Command line** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`</span></span>
     * <span data-ttu-id="4fee2-315">**Arbetskatalogen** -/ openfoam/sloshingTank3D</span><span class="sxs-lookup"><span data-stu-id="4fee2-315">**Working directory** - /openfoam/sloshingTank3D</span></span>
7. <span data-ttu-id="4fee2-316">Lägga till beroenden toothese aktiviteter i stigande ordning.</span><span class="sxs-lookup"><span data-stu-id="4fee2-316">Add dependencies toothese tasks in ascending task order.</span></span>
   
   ![Aktivitetsberoenden][task_dependencies]
8. <span data-ttu-id="4fee2-318">Klicka på **skicka** toorun jobbet.</span><span class="sxs-lookup"><span data-stu-id="4fee2-318">Click **Submit** toorun this job.</span></span>
   
   <span data-ttu-id="4fee2-319">Som standard skickar HPC Pack hello jobb som din aktuella inloggade användarkontot.</span><span class="sxs-lookup"><span data-stu-id="4fee2-319">By default, HPC Pack submits hello job as your current logged-on user account.</span></span> <span data-ttu-id="4fee2-320">När du klickar på **skicka**, du kan se en dialogruta visas där du tooenter hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="4fee2-320">After you click **Submit**, you might see a dialog box prompting you tooenter hello user name and password.</span></span>
   
   ![Jobbet autentiseringsuppgifter][creds]
   
   <span data-ttu-id="4fee2-322">Under vissa förhållanden HPC Pack kommer ihåg hello användarinformation du inkommande innan och inte visa den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4fee2-322">Under some conditions, HPC Pack remembers hello user information you input before and doesn’t show this dialog box.</span></span> <span data-ttu-id="4fee2-323">toomake HPC Pack visa igen, ange hello följande kommando i Kommandotolken och sedan skicka hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="4fee2-323">toomake HPC Pack show it again, enter hello following command at a Command prompt and then submit hello job.</span></span>
   
   ```
   hpccred delcreds
   ```
9. <span data-ttu-id="4fee2-324">hello jobbet tar från flera minuter tooseveral timmar enligt toohello parametrar som du har angett för hello exemplet.</span><span class="sxs-lookup"><span data-stu-id="4fee2-324">hello job takes from tens of minutes tooseveral hours according toohello parameters you have set for hello sample.</span></span> <span data-ttu-id="4fee2-325">Hello termisk karta visas i hello jobb som körs på hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="4fee2-325">In hello heat map, you see hello job running on hello Linux nodes.</span></span> 
   
   ![Termisk karta][heat_map]
   
   <span data-ttu-id="4fee2-327">På varje nod startas åtta processer.</span><span class="sxs-lookup"><span data-stu-id="4fee2-327">On each node, eight processes are started.</span></span>
   
   ![Linux-processer][linux_processes]
10. <span data-ttu-id="4fee2-329">Hitta hello jobbet resultat i mappar under C:\OpenFoam\sloshingTank3D och hello-loggfilerna på C:\OpenFoam när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4fee2-329">When hello job finishes, find hello job results in folders under C:\OpenFoam\sloshingTank3D, and hello log files at C:\OpenFoam.</span></span>

## <a name="view-results-in-ensight"></a><span data-ttu-id="4fee2-330">Visa resultat i EnSight</span><span class="sxs-lookup"><span data-stu-id="4fee2-330">View results in EnSight</span></span>
<span data-ttu-id="4fee2-331">Du kan också använda [EnSight](https://www.ceisoftware.com/) toovisualize och analysera hello resultatet av hello OpenFOAM jobbet.</span><span class="sxs-lookup"><span data-stu-id="4fee2-331">Optionally use [EnSight](https://www.ceisoftware.com/) toovisualize and analyze hello results of hello OpenFOAM job.</span></span> <span data-ttu-id="4fee2-332">Finns det mer information om visualisering och animeringen i EnSight [video guiden](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span><span class="sxs-lookup"><span data-stu-id="4fee2-332">For more about visualization and animation in EnSight, see this [video guide](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).</span></span>

1. <span data-ttu-id="4fee2-333">När du har installerat EnSight på hello huvudnod startar du den.</span><span class="sxs-lookup"><span data-stu-id="4fee2-333">After you install EnSight on hello head node, start it.</span></span>
2. <span data-ttu-id="4fee2-334">Öppna C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span><span class="sxs-lookup"><span data-stu-id="4fee2-334">Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.</span></span>
   
   <span data-ttu-id="4fee2-335">Du kan se en behållare i hello viewer.</span><span class="sxs-lookup"><span data-stu-id="4fee2-335">You see a tank in hello viewer.</span></span>
   
   ![Tanken i EnSight][tank]
3. <span data-ttu-id="4fee2-337">Skapa en **Isosurface** från **internalMesh**, och välj sedan hello variabeln **alpha_water**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-337">Create an **Isosurface** from **internalMesh**, and then choose hello variable **alpha_water**.</span></span>
   
   ![Skapa en isosurface][isosurface]
4. <span data-ttu-id="4fee2-339">Ange hello färg för **Isosurface_part** skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-339">Set hello color for **Isosurface_part** created in hello previous step.</span></span> <span data-ttu-id="4fee2-340">Till exempel den toowater blå.</span><span class="sxs-lookup"><span data-stu-id="4fee2-340">For example, set it toowater blue.</span></span>
   
   ![Redigera isosurface färg][isosurface_color]
5. <span data-ttu-id="4fee2-342">Skapa en **Iso-volym** från **väggar** genom att välja **väggar** i hello **delar** panelen och klicka på hello **Isosurfaces**  knapp i toolbar hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-342">Create an **Iso-volume** from **walls** by selecting **walls** in hello **Parts** panel and click hello **Isosurfaces** button in hello toolbar.</span></span>
6. <span data-ttu-id="4fee2-343">Hello i dialogrutan Välj **typen** som **Isovolume** och ange hello Min för **Isovolume intervallet** too0.5.</span><span class="sxs-lookup"><span data-stu-id="4fee2-343">In hello dialog box, select **Type** as **Isovolume** and set hello Min of **Isovolume range** too0.5.</span></span> <span data-ttu-id="4fee2-344">toocreate Hej isovolume, klickar du på **skapa med valda delar**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-344">toocreate hello isovolume, click **Create with selected parts**.</span></span>
7. <span data-ttu-id="4fee2-345">Ange hello färg för **Iso_volume_part** skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="4fee2-345">Set hello color for **Iso_volume_part** created in hello previous step.</span></span> <span data-ttu-id="4fee2-346">Till exempel den toodeep vattenstämplar blå.</span><span class="sxs-lookup"><span data-stu-id="4fee2-346">For example, set it toodeep water blue.</span></span>
8. <span data-ttu-id="4fee2-347">Ange hello färg för **väggar**.</span><span class="sxs-lookup"><span data-stu-id="4fee2-347">Set hello color for **walls**.</span></span> <span data-ttu-id="4fee2-348">Till exempel den tootransparent vit.</span><span class="sxs-lookup"><span data-stu-id="4fee2-348">For example, set it tootransparent white.</span></span>
9. <span data-ttu-id="4fee2-349">Klicka på **spela upp** toosee hello resultaten av hello simuleringen.</span><span class="sxs-lookup"><span data-stu-id="4fee2-349">Now click **Play** toosee hello results of hello simulation.</span></span>
   
    ![Tanken resultat][tank_result]

## <a name="sample-files"></a><span data-ttu-id="4fee2-351">Exempelfiler</span><span class="sxs-lookup"><span data-stu-id="4fee2-351">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="4fee2-352">Exempel XML-konfigurationsfilen för kluster-distributionen med PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="4fee2-352">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a><span data-ttu-id="4fee2-353">Exempelfilen cred.xml</span><span class="sxs-lookup"><span data-stu-id="4fee2-353">Sample cred.xml file</span></span>
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-tooinstall-mpi"></a><span data-ttu-id="4fee2-354">Exempel på silent.cfg filen tooinstall MPI</span><span class="sxs-lookup"><span data-stu-id="4fee2-354">Sample silent.cfg file tooinstall MPI</span></span>
```
# Patterns used toocheck silent configuration file
#
# anythingpat - any string
# filepat     - hello file location pattern (/file/location/to/license.lic)
# lspat       - hello license server address pattern (0123@hostname)
# snpat       - hello serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components tooinstall, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path toohello cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes tooenable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes tooupdate ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a><span data-ttu-id="4fee2-355">Exempelskript för settings.sh</span><span class="sxs-lookup"><span data-stu-id="4fee2-355">Sample settings.sh script</span></span>
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a><span data-ttu-id="4fee2-356">Exempelskript för hpcimpirun.sh</span><span class="sxs-lookup"><span data-stu-id="4fee2-356">Sample hpcimpirun.sh script</span></span>
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run hello mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into hello hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
