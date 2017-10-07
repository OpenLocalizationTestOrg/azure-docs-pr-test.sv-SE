---
title: "aaaNAMD with Microsoft HPC Pack på virtuella Linux-datorer | Microsoft Docs"
description: "Distribuera ett Microsoft HPC Pack kluster på Azure och köra en NAMD simulering med charmrun på flera Linux compute-noder"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: 90c722f66b494335a62c0613079366ae99002076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a><span data-ttu-id="727ff-103">Kör NAMD med Microsoft HPC Pack på beräkningsnoder för Linux i Azure</span><span class="sxs-lookup"><span data-stu-id="727ff-103">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>
<span data-ttu-id="727ff-104">Den här artikeln visar enkelriktade toorun en Linux högpresterande datorbearbetning (HPC) arbetsbelastningen på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="727ff-104">This article shows you one way toorun a Linux high-performance computing (HPC) workload on Azure virtual machines.</span></span> <span data-ttu-id="727ff-105">Här kan du ställa in en [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) på Azure-kluster med beräkningsnoder för Linux och köra en [NAMD](http://www.ks.uiuc.edu/Research/namd/) simuleringen toocalculate och visualisera hello struktur för ett stort biomolecular system.</span><span class="sxs-lookup"><span data-stu-id="727ff-105">Here, you set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster on Azure with Linux compute nodes and run a [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulation toocalculate and visualize hello structure of a large biomolecular system.</span></span>  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* <span data-ttu-id="727ff-106">**NAMD** (för Nanoscale molekylära Dynamics program) är en parallell molekylära dynamics paket designat för högpresterande simulering av stora biomolecular system som innehåller in toomillions av atomer.</span><span class="sxs-lookup"><span data-stu-id="727ff-106">**NAMD** (for Nanoscale Molecular Dynamics program) is a parallel molecular dynamics package designed for high-performance simulation of large biomolecular systems containing up toomillions of atoms.</span></span> <span data-ttu-id="727ff-107">Exempel på dessa system är virus, cell strukturer och stora protein.</span><span class="sxs-lookup"><span data-stu-id="727ff-107">Examples of these systems include viruses, cell structures, and large proteins.</span></span> <span data-ttu-id="727ff-108">NAMD skalas toohundreds kärnor för vanliga simulering och toomore än 500 000 kärnor för hello största simulering.</span><span class="sxs-lookup"><span data-stu-id="727ff-108">NAMD scales toohundreds of cores for typical simulations and toomore than 500,000 cores for hello largest simulations.</span></span>
* <span data-ttu-id="727ff-109">**Microsoft HPC Pack** ger funktioner toorun storskaliga HPC och parallella program i kluster av lokala datorer eller virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="727ff-109">**Microsoft HPC Pack** provides features toorun large-scale HPC and parallel applications in clusters of on-premises computers or Azure virtual machines.</span></span> <span data-ttu-id="727ff-110">Ursprungligen utvecklades som en lösning för Windows HPC-arbetsbelastningar, HPC Pack nu stöder som kör Linux HPC-program på Linux compute-nod virtuella datorer som distribueras i ett HPC Pack-kluster.</span><span class="sxs-lookup"><span data-stu-id="727ff-110">Originally developed as a solution for Windows HPC workloads, HPC Pack now supports running Linux HPC applications on Linux compute node VMs deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="727ff-111">Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) en introduktion.</span><span class="sxs-lookup"><span data-stu-id="727ff-111">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for an introduction.</span></span>

<span data-ttu-id="727ff-112">För andra alternativ toorun Linux HPC arbetsbelastningar i Azure, se [tekniska resurser för batch- och högpresterande datorbearbetning](../../../batch/batch-hpc-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="727ff-112">For other options toorun Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/batch-hpc-solutions.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="727ff-113">Krav</span><span class="sxs-lookup"><span data-stu-id="727ff-113">Prerequisites</span></span>
* <span data-ttu-id="727ff-114">**HPC Pack kluster med Linux datornoder** -distribuera ett kluster med HPC Pack med Linux compute-noder på Azure med hjälp av antingen en [Azure Resource Manager-mall](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) eller en [Azure PowerShell-skriptet](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="727ff-114">**HPC Pack cluster with Linux compute nodes** - Deploy an HPC Pack cluster with Linux compute nodes on Azure using either an [Azure Resource Manager template](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) or an [Azure PowerShell script](hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="727ff-115">Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster.md) hello krav och anvisningar för något av alternativen.</span><span class="sxs-lookup"><span data-stu-id="727ff-115">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for hello prerequisites and steps for either option.</span></span> <span data-ttu-id="727ff-116">Om du väljer hello distributionsalternativ för PowerShell-skript finns i hello exempelkonfigurationsfilen i hello exempelfiler hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="727ff-116">If you choose hello PowerShell script deployment option, see hello sample configuration file in hello sample files at hello end of this article.</span></span> <span data-ttu-id="727ff-117">Den här filen konfigurerar ett Azure-baserade HPC Pack kluster som består av en Windows Server 2012 R2 huvudnod och fyra storlek stora CentOS 6.6 compute-noder.</span><span class="sxs-lookup"><span data-stu-id="727ff-117">This file configures an Azure-based HPC Pack cluster consisting of a Windows Server 2012 R2 head node and four size Large CentOS 6.6 compute nodes.</span></span> <span data-ttu-id="727ff-118">Anpassa den här filen behövs för din miljö.</span><span class="sxs-lookup"><span data-stu-id="727ff-118">Customize this file as needed for your environment.</span></span>
* <span data-ttu-id="727ff-119">**NAMD programvara och kursen filer** -hämta NAMD programvara för Linux från hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) plats (registrering krävs).</span><span class="sxs-lookup"><span data-stu-id="727ff-119">**NAMD software and tutorial files** - Download NAMD software for Linux from hello [NAMD](http://www.ks.uiuc.edu/Research/namd/) site (registration required).</span></span> <span data-ttu-id="727ff-120">Den här artikeln är baserad på NAMD version 2.10 och använder hello [Linux-x86_64 (64-bitars Intel/AMD med Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) Arkiv.</span><span class="sxs-lookup"><span data-stu-id="727ff-120">This article is based on NAMD version 2.10, and uses hello [Linux-x86_64 (64-bit Intel/AMD with Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archive.</span></span> <span data-ttu-id="727ff-121">Dessutom hämta hello [NAMD självstudiekursen filer](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span><span class="sxs-lookup"><span data-stu-id="727ff-121">Also download hello [NAMD tutorial files](http://www.ks.uiuc.edu/Training/Tutorials/#namd).</span></span> <span data-ttu-id="727ff-122">hello nedladdningar är .tar-filer, och du behöver ett Windows-verktyget tooextract hello-filer på hello klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="727ff-122">hello downloads are .tar files, and you need a Windows tool tooextract hello files on hello cluster head node.</span></span> <span data-ttu-id="727ff-123">tooextract hello filer, följ instruktionerna för hello senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="727ff-123">tooextract hello files, follow hello instructions later in this article.</span></span> 
* <span data-ttu-id="727ff-124">**VMD** (valfritt) – toosee hello resultatet från jobbet NAMD, hämta och installera hello molekylära visualiseringen programmet [VMD](http://www.ks.uiuc.edu/Research/vmd/) på en dator som du väljer.</span><span class="sxs-lookup"><span data-stu-id="727ff-124">**VMD** (optional) - toosee hello results of your NAMD job, download and install hello molecular visualization program [VMD](http://www.ks.uiuc.edu/Research/vmd/) on a computer of your choice.</span></span> <span data-ttu-id="727ff-125">hello aktuella versionen är 1.9.2.</span><span class="sxs-lookup"><span data-stu-id="727ff-125">hello current version is 1.9.2.</span></span> <span data-ttu-id="727ff-126">Se hello VMD hämta platsen tooget igång.</span><span class="sxs-lookup"><span data-stu-id="727ff-126">See hello VMD download site tooget started.</span></span>  

## <a name="set-up-mutual-trust-between-compute-nodes"></a><span data-ttu-id="727ff-127">Ställ in ömsesidigt förtroende mellan beräkningsnoder</span><span class="sxs-lookup"><span data-stu-id="727ff-127">Set up mutual trust between compute nodes</span></span>
<span data-ttu-id="727ff-128">Kör ett jobb mellan noder på flera Linux-noder kräver hello noder tootrust varandra (av **rsh** eller **ssh**).</span><span class="sxs-lookup"><span data-stu-id="727ff-128">Running a cross-node job on multiple Linux nodes requires hello nodes tootrust each other (by **rsh** or **ssh**).</span></span> <span data-ttu-id="727ff-129">När du skapar hello HPC Pack kluster med hello Microsoft HPC Pack IaaS distributionsskriptet konfigurerar hello skript automatiskt permanenta ömsesidigt förtroende för hello administratörskontot som du anger.</span><span class="sxs-lookup"><span data-stu-id="727ff-129">When you create hello HPC Pack cluster with hello Microsoft HPC Pack IaaS deployment script, hello script automatically sets up permanent mutual trust for hello administrator account you specify.</span></span> <span data-ttu-id="727ff-130">Vanliga användare som du skapar i hello klustrets domän, har du tooset tillfälliga ömsesidigt förtroende mellan hello noder när ett jobb har allokerats toothem.</span><span class="sxs-lookup"><span data-stu-id="727ff-130">For non-administrator users you create in hello cluster's domain, you have tooset up temporary mutual trust among hello nodes when a job is allocated toothem.</span></span> <span data-ttu-id="727ff-131">Förstör hello relationen när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="727ff-131">Then, destroy hello relationship after hello job is complete.</span></span> <span data-ttu-id="727ff-132">toodo detta för varje användare måste ange ett RSA-nyckelpar toohello kluster vilka HPC Pack använder tooestablish hello förtroenderelation.</span><span class="sxs-lookup"><span data-stu-id="727ff-132">toodo this for each user, provide an RSA key pair toohello cluster which HPC Pack uses tooestablish hello trust relationship.</span></span> <span data-ttu-id="727ff-133">Följ instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="727ff-133">Instructions follow.</span></span>

### <a name="generate-an-rsa-key-pair"></a><span data-ttu-id="727ff-134">Generera en RSA-nyckelpar</span><span class="sxs-lookup"><span data-stu-id="727ff-134">Generate an RSA key pair</span></span>
<span data-ttu-id="727ff-135">Det är enkelt toogenerate ett RSA-nyckelpar, som innehåller en offentlig nyckel och en privat nyckel, genom att köra hello Linux **ssh-keygen** kommando.</span><span class="sxs-lookup"><span data-stu-id="727ff-135">It's easy toogenerate an RSA key pair, which contains a public key and a private key, by running hello Linux **ssh-keygen** command.</span></span>

1. <span data-ttu-id="727ff-136">Logga in tooa Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="727ff-136">Log on tooa Linux computer.</span></span>
2. <span data-ttu-id="727ff-137">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="727ff-137">Run hello following command:</span></span>
   
   ```bash
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > <span data-ttu-id="727ff-138">Tryck på **RETUR** toouse hello-standardinställningarna tills hello-kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="727ff-138">Press **Enter** toouse hello default settings until hello command is completed.</span></span> <span data-ttu-id="727ff-139">Ange inte en lösenfras här. När du uppmanas att ange ett lösenord trycker du bara på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="727ff-139">Do not enter a passphrase here; when prompted for a password, just press **Enter**.</span></span>
   > 
   > 
   
   ![Generera en RSA-nyckelpar][keygen]
3. <span data-ttu-id="727ff-141">Ändra katalogen toohello ~/.ssh katalog.</span><span class="sxs-lookup"><span data-stu-id="727ff-141">Change directory toohello ~/.ssh directory.</span></span> <span data-ttu-id="727ff-142">hello privata nyckeln lagras i id_rsa och hello offentliga nyckeln i id_rsa.pub.</span><span class="sxs-lookup"><span data-stu-id="727ff-142">hello private key is stored in id_rsa and hello public key in id_rsa.pub.</span></span>
   
   ![Privata och offentliga nycklar][keys]

### <a name="add-hello-key-pair-toohello-hpc-pack-cluster"></a><span data-ttu-id="727ff-144">Lägga till hello nyckelpar toohello HPC Pack kluster</span><span class="sxs-lookup"><span data-stu-id="727ff-144">Add hello key pair toohello HPC Pack cluster</span></span>
1. <span data-ttu-id="727ff-145">[Ansluta med Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello huvudnod VM med hello autentiseringsuppgifter för domänen som du angav när du har distribuerat hello kluster (till exempel hpc\clusteradmin).</span><span class="sxs-lookup"><span data-stu-id="727ff-145">[Connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello head node VM using hello domain credentials you provided when you deployed hello cluster (for example, hpc\clusteradmin).</span></span> <span data-ttu-id="727ff-146">Du kan hantera hello kluster från hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="727ff-146">You manage hello cluster from hello head node.</span></span>
2. <span data-ttu-id="727ff-147">Använd standard Windows Server procedurer toocreate ett domänanvändarkonto i hello klustrets Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="727ff-147">Use standard Windows Server procedures toocreate a domain user account in hello cluster's Active Directory domain.</span></span> <span data-ttu-id="727ff-148">Till exempel använda hello Active Directory-användare och datorer verktyget i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="727ff-148">For example, use hello Active Directory User and Computers tool on hello head node.</span></span> <span data-ttu-id="727ff-149">hello exemplen i den här artikeln förutsätter att du skapar en domänanvändare med namnet hpcuser i hello hpclab domän (hpclab\hpcuser).</span><span class="sxs-lookup"><span data-stu-id="727ff-149">hello examples in this article assume you create a domain user named hpcuser in hello hpclab domain (hpclab\hpcuser).</span></span>
3. <span data-ttu-id="727ff-150">Lägga till hello domän användaren toohello HPC Pack klustret som en användare i klustret.</span><span class="sxs-lookup"><span data-stu-id="727ff-150">Add hello domain user toohello HPC Pack cluster as a cluster user.</span></span> <span data-ttu-id="727ff-151">Instruktioner finns i [Lägg till eller ta bort klustret användare](https://technet.microsoft.com/library/ff919330.aspx).</span><span class="sxs-lookup"><span data-stu-id="727ff-151">For instructions, see [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).</span></span>
4. <span data-ttu-id="727ff-152">Skapa en fil med namnet C:\cred.xml och kopiera hello RSA viktiga data till den.</span><span class="sxs-lookup"><span data-stu-id="727ff-152">Create a file named C:\cred.xml and copy hello RSA key data into it.</span></span> <span data-ttu-id="727ff-153">Du hittar ett exempel i hello exempelfiler hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="727ff-153">You can find an example in hello sample files at hello end of this article.</span></span>
   
   ```
   <ExtendedData>
     <PrivateKey>Copy hello contents of private key here</PrivateKey>
     <PublicKey>Copy hello contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. <span data-ttu-id="727ff-154">Öppna en kommandotolk och ange följande kommando tooset hello autentiseringsuppgifter data för hello hpclab\hpcuser konto hello.</span><span class="sxs-lookup"><span data-stu-id="727ff-154">Open a Command Prompt and enter hello following command tooset hello credentials data for hello hpclab\hpcuser account.</span></span> <span data-ttu-id="727ff-155">Du använder hello **extendeddata** parametern toopass hello namnet hello C:\cred.xml fil du skapade hello viktiga data.</span><span class="sxs-lookup"><span data-stu-id="727ff-155">You use hello **extendeddata** parameter toopass hello name of hello C:\cred.xml file you created for hello key data.</span></span>
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   <span data-ttu-id="727ff-156">Det här kommandot har slutförts utan utdata.</span><span class="sxs-lookup"><span data-stu-id="727ff-156">This command completes successfully without output.</span></span> <span data-ttu-id="727ff-157">Lagra hello cred.xml filen på en säker plats när du har angett hello autentiseringsuppgifter för hello användarkonton som du behöver toorun jobb, eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="727ff-157">After setting hello credentials for hello user accounts you need toorun jobs, store hello cred.xml file in a secure location, or delete it.</span></span>
6. <span data-ttu-id="727ff-158">Om du har genererat hello RSA-nyckelpar på en Linux-noder kan du komma ihåg toodelete hello nycklar när du är klar med dem..</span><span class="sxs-lookup"><span data-stu-id="727ff-158">If you generated hello RSA key pair on one of your Linux nodes, remember toodelete hello keys after you finish using them.</span></span> <span data-ttu-id="727ff-159">HPC Pack inte ställts in ömsesidigt förtroende om den hittar en befintlig id_rsa- eller id_rsa.pub-fil.</span><span class="sxs-lookup"><span data-stu-id="727ff-159">HPC Pack does not set up mutual trust if it finds an existing id_rsa file or id_rsa.pub file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="727ff-160">Vi rekommenderar inte kör ett Linux-jobb som en Klusteradministratör i ett delat kluster, eftersom ett jobb som skickats av en administratör köras under rotkontot hello på hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="727ff-160">We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under hello root account on hello Linux nodes.</span></span> <span data-ttu-id="727ff-161">Ett jobb som skickats av en icke-administratörer körs under ett lokalt användarkonto för Linux med hello samma namn som hello jobbet användare.</span><span class="sxs-lookup"><span data-stu-id="727ff-161">A job submitted by a non-administrator user runs under a local Linux user account with hello same name as hello job user.</span></span> <span data-ttu-id="727ff-162">I det här fallet konfigurerar HPC Pack ömsesidigt förtroende för den här Linux användaren alla hello noder allokeras toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="727ff-162">In this case, HPC Pack sets up mutual trust for this Linux user across all hello nodes allocated toohello job.</span></span> <span data-ttu-id="727ff-163">Du kan ställa in hello Linux användare manuellt på hello Linux noder innan du kör jobbet hello eller HPC Pack skapar hello användaren automatiskt när hello jobbet har skickats.</span><span class="sxs-lookup"><span data-stu-id="727ff-163">You can set up hello Linux user manually on hello Linux nodes before running hello job, or HPC Pack creates hello user automatically when hello job is submitted.</span></span> <span data-ttu-id="727ff-164">Om HPC Pack skapar hello användare, HPC Pack tar bort den efter hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="727ff-164">If HPC Pack creates hello user, HPC Pack deletes it after hello job completes.</span></span> <span data-ttu-id="727ff-165">tooreduce säkerhetshot, hello nycklarna tas bort när hello jobbet har slutförts på hello noder.</span><span class="sxs-lookup"><span data-stu-id="727ff-165">tooreduce security threat, hello keys are removed after hello job completes on hello nodes.</span></span>
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a><span data-ttu-id="727ff-166">Skapa en filresurs för Linux-noder</span><span class="sxs-lookup"><span data-stu-id="727ff-166">Set up a file share for Linux nodes</span></span>
<span data-ttu-id="727ff-167">Nu konfigurera en SMB-filresurs och montera hello delad mapp på alla Linux noder tooallow hello Linux noder tooaccess NAMD filer med en sökväg.</span><span class="sxs-lookup"><span data-stu-id="727ff-167">Now set up an SMB file share, and mount hello shared folder on all Linux nodes tooallow hello Linux nodes tooaccess NAMD files with a common path.</span></span> <span data-ttu-id="727ff-168">Nedan följer stegen toomount en delad mapp på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="727ff-168">Following are steps toomount a shared folder on hello head node.</span></span> <span data-ttu-id="727ff-169">En resurs som rekommenderas för distributioner, till exempel CentOS 6.6 som för närvarande inte stöder hello Azure File service.</span><span class="sxs-lookup"><span data-stu-id="727ff-169">A share is recommended for distributions such as CentOS 6.6 that currently don’t support hello Azure File service.</span></span> <span data-ttu-id="727ff-170">Om Linux-noder stöder en Azure-filresurs, se [hur toouse Azure File storage med Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="727ff-170">If your Linux nodes support an Azure File share, see [How toouse Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="727ff-171">För ytterligare alternativ för delning med HPC Pack-fil, se [komma igång med Linux compute-noder i ett HPC Pack-kluster i Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="727ff-171">For additional file sharing options with HPC Pack, see [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](hpcpack-cluster.md).</span></span>

1. <span data-ttu-id="727ff-172">Skapa en mapp på hello huvudnod och dela den tooEveryone genom att ange behörighet för läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="727ff-172">Create a folder on hello head node, and share it tooEveryone by setting Read/Write privileges.</span></span> <span data-ttu-id="727ff-173">I det här exemplet \\ \\CentOS66HN\Namd är hello namnet på hello mapp där CentOS66HN är hello värdnamnet för hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="727ff-173">In this example, \\\\CentOS66HN\Namd is hello name of hello folder, where CentOS66HN is hello host name of hello head node.</span></span>
2. <span data-ttu-id="727ff-174">Skapa en undermapp som heter namd2 i hello delad mapp.</span><span class="sxs-lookup"><span data-stu-id="727ff-174">Create a subfolder named namd2 in hello shared folder.</span></span> <span data-ttu-id="727ff-175">Skapa en annan undermapp som heter namdsample i namd2.</span><span class="sxs-lookup"><span data-stu-id="727ff-175">In namd2, create another subfolder named namdsample.</span></span>
3. <span data-ttu-id="727ff-176">Extrahera hello NAMD filer i mappen hello med hjälp av en Windows-versionen av **tar** eller något annat verktyg i Windows som körs på .tar Arkiv.</span><span class="sxs-lookup"><span data-stu-id="727ff-176">Extract hello NAMD files in hello folder by using a Windows version of **tar** or another Windows utility that operates on .tar archives.</span></span> 
   
   * <span data-ttu-id="727ff-177">Extrahera hello NAMD tar Arkiv för\\\\CentOS66HN\Namd\namd2.</span><span class="sxs-lookup"><span data-stu-id="727ff-177">Extract hello NAMD tar archive too\\\\CentOS66HN\Namd\namd2.</span></span>
   * <span data-ttu-id="727ff-178">Extrahera hello självstudiekursen filer under \\ \\CentOS66HN\Namd\namd2\namdsample.</span><span class="sxs-lookup"><span data-stu-id="727ff-178">Extract hello tutorial files under \\\\CentOS66HN\Namd\namd2\namdsample.</span></span>
4. <span data-ttu-id="727ff-179">Öppna Windows PowerShell-fönstret och kör följande kommandon toomount hello delad mapp på hello Linux noder hello.</span><span class="sxs-lookup"><span data-stu-id="727ff-179">Open a Windows PowerShell window and run hello following commands toomount hello shared folder on hello Linux nodes.</span></span>
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="727ff-180">hello första kommandot skapar en mapp med namnet /namd2 på alla noder i hello LinuxNodes grupp.</span><span class="sxs-lookup"><span data-stu-id="727ff-180">hello first command creates a folder named /namd2 on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="727ff-181">hello andra kommandot monterar hello delade mappen //CentOS66HN/Namd/namd2 till hello mapp med dir_mode och file_mode bits set too777.</span><span class="sxs-lookup"><span data-stu-id="727ff-181">hello second command mounts hello shared folder //CentOS66HN/Namd/namd2 onto hello folder with dir_mode and file_mode bits set too777.</span></span> <span data-ttu-id="727ff-182">Hej *användarnamn* och *lösenord* hello kommandot ska vara hello autentiseringsuppgifterna för en användare i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="727ff-182">hello *username* and *password* in hello command should be hello credentials of a user on hello head node.</span></span>

> [!NOTE]
> <span data-ttu-id="727ff-183">Hej ”\\`” symbol i andra hello-kommandot är en symbolen för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="727ff-183">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="727ff-184">”\\`,” innebär hello ””, (kommatecken tecken) är en del av hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="727ff-184">“\\`,” means hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

## <a name="create-a-bash-script-toorun-a-namd-job"></a><span data-ttu-id="727ff-185">Skapa ett Bash-skript toorun ett NAMD-jobb</span><span class="sxs-lookup"><span data-stu-id="727ff-185">Create a Bash script toorun a NAMD job</span></span>
<span data-ttu-id="727ff-186">Jobbet NAMD måste en *nodelist* för **charmrun** toodetermine hello antalet noder toouse när du startar NAMD processer.</span><span class="sxs-lookup"><span data-stu-id="727ff-186">Your NAMD job needs a *nodelist* file for **charmrun** toodetermine hello number of nodes toouse when starting NAMD processes.</span></span> <span data-ttu-id="727ff-187">Du använder ett Bash-skript som genererar hello nodlistfilen och kör **charmrun** med den här nodlistfilen.</span><span class="sxs-lookup"><span data-stu-id="727ff-187">You use a Bash script that generates hello nodelist file and runs **charmrun** with this nodelist file.</span></span> <span data-ttu-id="727ff-188">Du kan sedan skicka ett NAMD jobb i HPC Cluster Manager som anropar det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="727ff-188">You can then submit a NAMD job in HPC Cluster Manager that calls this script.</span></span>

<span data-ttu-id="727ff-189">Med hjälp av en textredigerare önskat, skapa ett Bash-skript i hello /namd2 mapp hello NAMD programfiler och ger den namnet hpccharmrun.sh. Kopiera hello hpccharmrun.sh exempelskript tillhandahålls hello slutet av den här artikeln för en snabb konceptbevis och gå för[skickar ett jobb för NAMD](#submit-a-namd-job).</span><span class="sxs-lookup"><span data-stu-id="727ff-189">Using a text editor of your choice, create a Bash script in hello /namd2 folder containing hello NAMD program files and name it hpccharmrun.sh. For a quick proof of concept, copy hello example hpccharmrun.sh script provided at hello end of this article and go too[Submit a NAMD job](#submit-a-namd-job).</span></span>

> [!TIP]
> <span data-ttu-id="727ff-190">Spara skriptet som en textfil med Linux radbrytningar (endast LF, inte CR LF).</span><span class="sxs-lookup"><span data-stu-id="727ff-190">Save your script as a text file with Linux line endings (LF only, not CR LF).</span></span> <span data-ttu-id="727ff-191">Detta säkerställer att den körs korrekt på hello Linux noder.</span><span class="sxs-lookup"><span data-stu-id="727ff-191">This ensures that it runs properly on hello Linux nodes.</span></span>
> 
> 

<span data-ttu-id="727ff-192">Nedan visas information om den här bash-skript har.</span><span class="sxs-lookup"><span data-stu-id="727ff-192">Following are details about what this bash script does.</span></span> 

1. <span data-ttu-id="727ff-193">Definiera några variabler.</span><span class="sxs-lookup"><span data-stu-id="727ff-193">Define some variables.</span></span>
   
   ```bash
   #!/bin/bash
   
   # hello path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. <span data-ttu-id="727ff-194">Hämta information om noden från hello miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="727ff-194">Get node information from hello environment variables.</span></span> <span data-ttu-id="727ff-195">$NODESCORES lagras en lista över delade ord från $CCP_NODES_CORES.</span><span class="sxs-lookup"><span data-stu-id="727ff-195">$NODESCORES stores a list of split words from $CCP_NODES_CORES.</span></span> <span data-ttu-id="727ff-196">$COUNT är hello storleken på $NODESCORES.</span><span class="sxs-lookup"><span data-stu-id="727ff-196">$COUNT is hello size of $NODESCORES.</span></span>
   
   ```
   # Get node information from hello environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   <span data-ttu-id="727ff-197">hello format för hello $CCP_NODES_CORES variabeln är följande:</span><span class="sxs-lookup"><span data-stu-id="727ff-197">hello format for hello $CCP_NODES_CORES variable is as follows:</span></span>
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   <span data-ttu-id="727ff-198">Den här variabeln visar hello totala antalet noder, nodnamn och antalet kärnor på varje nod som är allokerade toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="727ff-198">This variable lists hello total number of nodes, node names, and number of cores on each node that are allocated toohello job.</span></span> <span data-ttu-id="727ff-199">Till exempel om hello jobbet måste 10 kärnor toorun, liknar hello värdet för $CCP_NODES_CORES:</span><span class="sxs-lookup"><span data-stu-id="727ff-199">For example, if hello job needs 10 cores toorun, hello value of $CCP_NODES_CORES is similar to:</span></span>
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. <span data-ttu-id="727ff-200">Starta om hello $CCP_NODES_CORES variabeln inte anges **charmrun** direkt.</span><span class="sxs-lookup"><span data-stu-id="727ff-200">If hello $CCP_NODES_CORES variable is not set, start **charmrun** directly.</span></span> <span data-ttu-id="727ff-201">(Detta bör endast inträffa när du kör det här skriptet direkt på Linux-noder.)</span><span class="sxs-lookup"><span data-stu-id="727ff-201">(This should only occur when you run this script directly on your Linux nodes.)</span></span>
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. <span data-ttu-id="727ff-202">Eller skapa en nodlistfilen för **charmrun**.</span><span class="sxs-lookup"><span data-stu-id="727ff-202">Or create a nodelist file for **charmrun**.</span></span>
   
   ```
   else
     # Create hello nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write hello head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into hello nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. <span data-ttu-id="727ff-203">Kör **charmrun** med hello nodlistfilen hämta dess returstatus och ta bort hello nodlistfilen hello slutet.</span><span class="sxs-lookup"><span data-stu-id="727ff-203">Run **charmrun** with hello nodelist file, get its return status, and remove hello nodelist file at hello end.</span></span>
   
   <span data-ttu-id="727ff-204">${CCP_NUMCPUS} är en annan miljövariabel som angetts av hello HPC Pack huvudnod.</span><span class="sxs-lookup"><span data-stu-id="727ff-204">${CCP_NUMCPUS} is another environment variable set by hello HPC Pack head node.</span></span> <span data-ttu-id="727ff-205">Lagrar den hello antal totala kärnor allokerade toothis jobb.</span><span class="sxs-lookup"><span data-stu-id="727ff-205">It stores hello number of total cores allocated toothis job.</span></span> <span data-ttu-id="727ff-206">Vi använder den toospecify hello antal processer för charmrun.</span><span class="sxs-lookup"><span data-stu-id="727ff-206">We use it toospecify hello number of processes for charmrun.</span></span>
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. <span data-ttu-id="727ff-207">Avsluta med hello **charmrun** returstatus.</span><span class="sxs-lookup"><span data-stu-id="727ff-207">Exit with hello **charmrun** return status.</span></span>
   
   ```
   exit ${RTNSTS}
   ```

<span data-ttu-id="727ff-208">Följande är hello nodlistfilen vilka hello skriptet genererar hello information:</span><span class="sxs-lookup"><span data-stu-id="727ff-208">Following is hello information in hello nodelist file, which hello script generates:</span></span>

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

<span data-ttu-id="727ff-209">Exempel:</span><span class="sxs-lookup"><span data-stu-id="727ff-209">For example:</span></span>

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a><span data-ttu-id="727ff-210">Skicka ett NAMD-jobb</span><span class="sxs-lookup"><span data-stu-id="727ff-210">Submit a NAMD job</span></span>
<span data-ttu-id="727ff-211">Nu är du redo toosubmit NAMD-jobb i HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="727ff-211">Now you are ready toosubmit a NAMD job in HPC Cluster Manager.</span></span>

1. <span data-ttu-id="727ff-212">Anslut tooyour klustrets huvudnod och starta HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="727ff-212">Connect tooyour cluster head node and start HPC Cluster Manager.</span></span>
2. <span data-ttu-id="727ff-213">I **resurshantering**, se till att hello Linux datornoderna hello **Online** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="727ff-213">In **Resource Management**, ensure that hello Linux compute nodes are in hello **Online** state.</span></span> <span data-ttu-id="727ff-214">Om du inte markerar du dem och klickar på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="727ff-214">If they are not, select them and click **Bring Online**.</span></span>
3. <span data-ttu-id="727ff-215">I **jobbhantering**, klickar du på **nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="727ff-215">In **Job Management**, click **New Job**.</span></span>
4. <span data-ttu-id="727ff-216">Ange ett namn för jobbet som *hpccharmrun*.</span><span class="sxs-lookup"><span data-stu-id="727ff-216">Enter a name for job such as *hpccharmrun*.</span></span>
   
   ![Nytt HPC-jobb][namd_job]
5. <span data-ttu-id="727ff-218">På hello **jobbinformation** sidan under **jobbet resurser**, Välj hello typ av resurs som **nod** och ange hello **minsta** too3.</span><span class="sxs-lookup"><span data-stu-id="727ff-218">On hello **Job Details** page, under **Job Resources**, select hello type of resource as **Node** and set hello **Minimum** too3.</span></span> <span data-ttu-id="727ff-219">, vi kör hello jobb på tre noder för Linux och varje nod har fyra kärnor.</span><span class="sxs-lookup"><span data-stu-id="727ff-219">, we run hello job on three Linux nodes and each node has four cores.</span></span>
   
   ![Jobbet resurser][job_resources]
6. <span data-ttu-id="727ff-221">Klicka på **redigera uppgifter** i hello vänstra navigeringsfönstret och klicka sedan på **Lägg till** tooadd en toohello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="727ff-221">Click **Edit Tasks** in hello left navigation, and then click **Add** tooadd a task toohello job.</span></span>    
7. <span data-ttu-id="727ff-222">På hello **information om aktiviteter och i/o-omdirigering** sidan Ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="727ff-222">On hello **Task Details and I/O Redirection** page, set hello following values:</span></span>
   
   * <span data-ttu-id="727ff-223">**Kommandoraden**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span><span class="sxs-lookup"><span data-stu-id="727ff-223">**Command line** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`</span></span>
     
     > [!TIP]
     > <span data-ttu-id="727ff-224">hello är föregående kommandoraden ett enda kommando utan radbrytningar.</span><span class="sxs-lookup"><span data-stu-id="727ff-224">hello preceding command line is a single command without line breaks.</span></span> <span data-ttu-id="727ff-225">Den radbryts tooappear på flera rader under **kommandoraden**.</span><span class="sxs-lookup"><span data-stu-id="727ff-225">It wraps tooappear on several lines under **Command line**.</span></span>
     > 
     > 
   * <span data-ttu-id="727ff-226">**Arbetskatalogen** -/namd2</span><span class="sxs-lookup"><span data-stu-id="727ff-226">**Working directory** - /namd2</span></span>
   * <span data-ttu-id="727ff-227">**Minsta** – 3</span><span class="sxs-lookup"><span data-stu-id="727ff-227">**Minimum** - 3</span></span>
     
     ![Uppgiftsinformation][task_details]
     
     > [!NOTE]
     > <span data-ttu-id="727ff-229">Du har angett hello arbetskatalogen här eftersom **charmrun** försöker toonavigate toohello samma arbetskatalogen på varje nod.</span><span class="sxs-lookup"><span data-stu-id="727ff-229">You set hello working directory here because **charmrun** tries toonavigate toohello same working directory on each node.</span></span> <span data-ttu-id="727ff-230">Om hello arbetskatalogen har inte angetts, startas HPC Pack hello-kommando i en mapp som skapats på ett hello Linux noder slumpmässigt namn.</span><span class="sxs-lookup"><span data-stu-id="727ff-230">If hello working directory isn't set, HPC Pack starts hello command in a randomly named folder created on one of hello Linux nodes.</span></span> <span data-ttu-id="727ff-231">Detta medför följande fel på hello hello andra noder: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid problemet, ange en mappsökväg som kan nås av alla noder som hello arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="727ff-231">This causes hello following error on hello other nodes: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` tooavoid this problem, specify a folder path that can be accessed by all nodes as hello working directory.</span></span>
     > 
     > 
8. <span data-ttu-id="727ff-232">Klicka på **OK** och klicka sedan på **skicka** toorun jobbet.</span><span class="sxs-lookup"><span data-stu-id="727ff-232">Click **OK** and then click **Submit** toorun this job.</span></span>
   
   <span data-ttu-id="727ff-233">Som standard skickar HPC Pack hello jobb som din aktuella inloggade användarkontot.</span><span class="sxs-lookup"><span data-stu-id="727ff-233">By default, HPC Pack submits hello job as your current logged-on user account.</span></span> <span data-ttu-id="727ff-234">En dialogruta blir du ombedd tooenter hello användarnamn och lösenord när du klickar på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="727ff-234">A dialog box might prompt you tooenter hello user name and password after you click **Submit**.</span></span>
   
   ![Jobbet autentiseringsuppgifter][creds]
   
   <span data-ttu-id="727ff-236">Under vissa förhållanden HPC Pack kommer ihåg hello användarinformation du inkommande innan och inte visa den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="727ff-236">Under some conditions, HPC Pack remembers hello user information you input before and doesn't show this dialog box.</span></span> <span data-ttu-id="727ff-237">toomake HPC Pack visa igen, ange hello följande kommando i Kommandotolken och sedan skicka hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="727ff-237">toomake HPC Pack show it again, enter hello following command at a Command Prompt and then submit hello job.</span></span>
   
   ```command
   hpccred delcreds
   ```
9. <span data-ttu-id="727ff-238">hello jobbet tar flera minuter toofinish.</span><span class="sxs-lookup"><span data-stu-id="727ff-238">hello job takes several minutes toofinish.</span></span>
10. <span data-ttu-id="727ff-239">Hitta hello jobblogg på \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log och hello utdatafiler i \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span><span class="sxs-lookup"><span data-stu-id="727ff-239">Find hello job log at \\<headnodeName>\Namd\namd2\namd2_hpccharmrun.log and hello output files in \\<headnodeName>\Namd\namd2\namdsample\1-2-sphere\.</span></span>
11. <span data-ttu-id="727ff-240">Du kan också starta VMD tooview jobbresultaten.</span><span class="sxs-lookup"><span data-stu-id="727ff-240">Optionally, start VMD tooview your job results.</span></span> <span data-ttu-id="727ff-241">hello steg för att visualisera hello NAMD utdatafilerna (i det här fallet en ubiquitin protein molekylen vattenstämplar gäller) ligger utanför hello omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="727ff-241">hello steps for visualizing hello NAMD output files (in this case, a ubiquitin protein molecule in a water sphere) are beyond hello scope of this article.</span></span> <span data-ttu-id="727ff-242">Se [NAMD kursen](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) mer information.</span><span class="sxs-lookup"><span data-stu-id="727ff-242">See [NAMD Tutorial](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) for details.</span></span>
    
    ![Jobbet resultat][vmd_view]

## <a name="sample-files"></a><span data-ttu-id="727ff-244">Exempelfiler</span><span class="sxs-lookup"><span data-stu-id="727ff-244">Sample files</span></span>
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a><span data-ttu-id="727ff-245">Exempel XML-konfigurationsfilen för kluster-distributionen med PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="727ff-245">Sample XML configuration file for cluster deployment by PowerShell script</span></span>
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a><span data-ttu-id="727ff-246">Exempelfilen cred.xml</span><span class="sxs-lookup"><span data-stu-id="727ff-246">Sample cred.xml file</span></span>
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

### <a name="sample-hpccharmrunsh-script"></a><span data-ttu-id="727ff-247">Exempelskript för hpccharmrun.sh</span><span class="sxs-lookup"><span data-stu-id="727ff-247">Sample hpccharmrun.sh script</span></span>
```
#!/bin/bash

# hello path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run hello charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create hello nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write hello head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into hello nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run hello charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
