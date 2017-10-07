---
title: "aaaResources för batch- och HPC i hello Azure-molnet | Microsoft Docs"
description: "Visar en lista över tekniska resurser toohelp du kör din storskaliga parallellt, batch och högpresterande datorbearbetning (HPC) arbetsbelastningar i Azure."
services: batch, cloud-services, virtual-machines
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 6f8be911-c841-41ae-88d3-3bcfc029eb7f
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 03/17/2017
ms.author: danlep
ms.openlocfilehash: ab8ba24678bd7ec090306b501d29ca63c4fb83ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a><span data-ttu-id="4e99e-103">Stora Compute i Azure: tekniska resurser för batch- och högpresterande datorbearbetning</span><span class="sxs-lookup"><span data-stu-id="4e99e-103">Big Compute in Azure: Technical resources for batch and high-performance computing</span></span>
<span data-ttu-id="4e99e-104">Det här är en guide tootechnical resurser toohelp som du kör din storskaliga parallellt, batch och högpresterande datorbearbetning (HPC) arbetsbelastningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="4e99e-104">This is a guide tootechnical resources toohelp you run your large-scale parallel, batch, and high-performance computing (HPC) workloads in Azure.</span></span> <span data-ttu-id="4e99e-105">Utöka din befintliga batch eller HPC arbetsbelastningar toohello Azure-molnet eller skapa nya stort Compute-lösningar med hjälp av ett intervall av Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4e99e-105">Extend your existing batch or HPC workloads toohello Azure cloud, or build new Big Compute solutions using a range of Azure services.</span></span>

## <a name="solutions-options"></a><span data-ttu-id="4e99e-106">Alternativ för lösningar</span><span class="sxs-lookup"><span data-stu-id="4e99e-106">Solutions options</span></span>
<span data-ttu-id="4e99e-107">Lär dig mer om stora Compute alternativ i Azure och välja hello rätt metod för din arbetsbelastning och företag behöver.</span><span class="sxs-lookup"><span data-stu-id="4e99e-107">Learn about Big Compute options in Azure, and choose hello right approach for your workload and business need.</span></span>

* [<span data-ttu-id="4e99e-108">Batch- och HPC-lösningar</span><span class="sxs-lookup"><span data-stu-id="4e99e-108">Batch and HPC solutions</span></span>](batch-hpc-solutions.md)
* [<span data-ttu-id="4e99e-109">Video: Stor beräkning i hello moln med Azure och HPC</span><span class="sxs-lookup"><span data-stu-id="4e99e-109">Video: Big Compute in hello cloud with Azure and HPC</span></span>](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)

## <a name="azure-batch"></a><span data-ttu-id="4e99e-110">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="4e99e-110">Azure Batch</span></span>
<span data-ttu-id="4e99e-111">[Batch](https://azure.microsoft.com/services/batch/) är en platform-tjänst som gör det enkelt toocloud – aktivera Linux och Windows-program och kör jobb utan att ställa in och hantera ett kluster och jobbet Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="4e99e-111">[Batch](https://azure.microsoft.com/services/batch/) is a platform service that makes it easy toocloud-enable your Linux and Windows applications and run jobs without setting up and managing a cluster and job scheduler.</span></span> <span data-ttu-id="4e99e-112">Använder hello SDK toointegrate program med Azure Batch via olika språk, mellanlagra data tooAzure och skapa pipelines för körning av jobbet.</span><span class="sxs-lookup"><span data-stu-id="4e99e-112">Use hello SDK toointegrate client applications with Azure Batch through various languages, stage data tooAzure, and build job execution pipelines.</span></span>

* [<span data-ttu-id="4e99e-113">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="4e99e-113">Documentation</span></span>](https://azure.microsoft.com/documentation/services/batch/)
* <span data-ttu-id="4e99e-114">[.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), och [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API-referens</span><span class="sxs-lookup"><span data-stu-id="4e99e-114">[.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), and [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API reference</span></span>
* <span data-ttu-id="4e99e-115">[Batch management .NET-bibliotek](https://msdn.microsoft.com/library/mt463120.aspx) referens</span><span class="sxs-lookup"><span data-stu-id="4e99e-115">[Batch management .NET library](https://msdn.microsoft.com/library/mt463120.aspx) reference</span></span>
* <span data-ttu-id="4e99e-116">Självstudier: Kom igång med [Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md) och [Batch Python-klient](batch-python-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="4e99e-116">Tutorials: Get started with [Azure Batch library for .NET](batch-dotnet-get-started.md) and [Batch Python client](batch-python-tutorial.md)</span></span>
* [<span data-ttu-id="4e99e-117">Batch-forum</span><span class="sxs-lookup"><span data-stu-id="4e99e-117">Batch forum</span></span>](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)
* [<span data-ttu-id="4e99e-118">Batch-videor</span><span class="sxs-lookup"><span data-stu-id="4e99e-118">Batch videos</span></span>](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a><span data-ttu-id="4e99e-119">HPC-kluster-lösningar</span><span class="sxs-lookup"><span data-stu-id="4e99e-119">HPC cluster solutions</span></span>
<span data-ttu-id="4e99e-120">Distribuera eller utöka din befintliga Windows eller Linux HPC-kluster tooAzure toorun beräknings-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="4e99e-120">Deploy or extend your existing Windows or Linux HPC cluster tooAzure toorun your compute intensive workloads.</span></span>  

### <a name="microsoft-hpc-pack"></a><span data-ttu-id="4e99e-121">Microsoft HPC Pack</span><span class="sxs-lookup"><span data-stu-id="4e99e-121">Microsoft HPC Pack</span></span>
<span data-ttu-id="4e99e-122">HPC Pack är Microsofts ledigt HPC-lösning baserat på Microsoft Azure och Windows Server-teknik kan köra Windows och Linux HPC arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="4e99e-122">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies, capable of running Windows and Linux HPC workloads.</span></span>  

* [<span data-ttu-id="4e99e-123">Hämta HPC Pack 2016</span><span class="sxs-lookup"><span data-stu-id="4e99e-123">Download HPC Pack 2016</span></span>](https://www.microsoft.com/download/details.aspx?id=54507)
* [<span data-ttu-id="4e99e-124">Hämta HPC Pack 2012 R2 uppdatering 3</span><span class="sxs-lookup"><span data-stu-id="4e99e-124">Download HPC Pack 2012 R2 Update 3</span></span>](https://www.microsoft.com/download/details.aspx?id=49922)
* [<span data-ttu-id="4e99e-125">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="4e99e-125">Documentation</span></span>](https://technet.microsoft.com/library/jj899572.aspx)
* <span data-ttu-id="4e99e-126">HPC Pack klustret alternativ i Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="4e99e-126">HPC Pack cluster options in Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) and [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span> 
* [<span data-ttu-id="4e99e-127">Burst tooAzure worker-instanser med HPC Pack</span><span class="sxs-lookup"><span data-stu-id="4e99e-127">Burst tooAzure worker instances with HPC Pack</span></span>](https://technet.microsoft.com/library/gg481749.aspx)
* [<span data-ttu-id="4e99e-128">TooAzure burst med HPC Pack</span><span class="sxs-lookup"><span data-stu-id="4e99e-128">Burst tooAzure  Batch with HPC Pack</span></span>](https://technet.microsoft.com/library/mt612877.aspx)
* [<span data-ttu-id="4e99e-129">Windows HPC-forum</span><span class="sxs-lookup"><span data-stu-id="4e99e-129">Windows HPC forums</span></span>](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a><span data-ttu-id="4e99e-130">Lösningar för Linux och OSS</span><span class="sxs-lookup"><span data-stu-id="4e99e-130">Linux and OSS cluster solutions</span></span>
<span data-ttu-id="4e99e-131">Använd dessa mallar för Azure-toodeploy Linux HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="4e99e-131">Use these Azure templates toodeploy Linux HPC clusters.</span></span>

* <span data-ttu-id="4e99e-132">[Få igång en SLURM klustret](https://azure.microsoft.com/documentation/templates/slurm/) och [blogginlägget](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="4e99e-132">[Spin up a SLURM cluster](https://azure.microsoft.com/documentation/templates/slurm/) and [blog post](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span></span>
* [<span data-ttu-id="4e99e-133">Få igång ett moment kluster</span><span class="sxs-lookup"><span data-stu-id="4e99e-133">Spin up a Torque cluster</span></span>](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [<span data-ttu-id="4e99e-134">Beräkna rutnätet mallar med PBS Professional</span><span class="sxs-lookup"><span data-stu-id="4e99e-134">Compute grid templates with PBS Professional</span></span>](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

## <a name="hpc-storage"></a><span data-ttu-id="4e99e-135">HPC-lagring</span><span class="sxs-lookup"><span data-stu-id="4e99e-135">HPC storage</span></span>
* [<span data-ttu-id="4e99e-136">Parallell filsystem för HPC-lagring på Azure</span><span class="sxs-lookup"><span data-stu-id="4e99e-136">Parallel file systems for HPC storage on Azure</span></span>](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)
* [<span data-ttu-id="4e99e-137">Intel molnet Edition för Lyster programvara - utvärdering</span><span class="sxs-lookup"><span data-stu-id="4e99e-137">Intel Cloud Edition for Lustre Software - Eval</span></span>](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)
* [<span data-ttu-id="4e99e-138">BeeGFS på CentOS 7.2 mall</span><span class="sxs-lookup"><span data-stu-id="4e99e-138">BeeGFS on CentOS 7.2 template</span></span>](https://github.com/smith1511/hpc/tree/master/beegfs-shared-on-centos7.2)




## <a name="microsoft-mpi"></a><span data-ttu-id="4e99e-139">Microsoft MPI</span><span class="sxs-lookup"><span data-stu-id="4e99e-139">Microsoft MPI</span></span>
<span data-ttu-id="4e99e-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) är en Microsoft-implementering av hello Message Passing Interface standard för att utveckla och parallella program som körs på Windows hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="4e99e-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) is a Microsoft implementation of hello Message Passing Interface standard for developing and running parallel applications on hello Windows platform.</span></span>

* [<span data-ttu-id="4e99e-141">Ladda ned MS-MPI</span><span class="sxs-lookup"><span data-stu-id="4e99e-141">Download MS-MPI</span></span>](http://go.microsoft.com/FWLink/p/?LinkID=389556)
* [<span data-ttu-id="4e99e-142">MS-MPI-referens</span><span class="sxs-lookup"><span data-stu-id="4e99e-142">MS-MPI reference</span></span>](https://msdn.microsoft.com/library/dn473458.aspx)
* [<span data-ttu-id="4e99e-143">MPI-forum</span><span class="sxs-lookup"><span data-stu-id="4e99e-143">MPI forum</span></span>](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a><span data-ttu-id="4e99e-144">Beräkningsintensiva instanser</span><span class="sxs-lookup"><span data-stu-id="4e99e-144">Compute-intensive instances</span></span>
<span data-ttu-id="4e99e-145">Azure erbjuder en [intervall för VM-storlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), inklusive [beräkningsintensiva H-serien](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instanser kan ansluta tooa RDMA serverdelsnätverk, toorun din Linux och Windows HPC-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="4e99e-145">Azure offers a [range of VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), including [compute-intensive H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instances capable of connecting tooa back-end RDMA network, toorun your Linux and Windows HPC workloads.</span></span> 

* [<span data-ttu-id="4e99e-146">Ställa in en Linux RDMA klustret toorun MPI-program</span><span class="sxs-lookup"><span data-stu-id="4e99e-146">Set up a Linux RDMA cluster toorun MPI applications</span></span>](../virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="4e99e-147">Konfigurera ett RDMA för Windows-kluster med Microsoft HPC Pack toorun MPI program</span><span class="sxs-lookup"><span data-stu-id="4e99e-147">Set up a Windows RDMA cluster with Microsoft HPC Pack toorun MPI applications</span></span>](../virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

<span data-ttu-id="4e99e-148">För GPU-intensiva arbetsbelastningar, kolla [NC-och NV](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span><span class="sxs-lookup"><span data-stu-id="4e99e-148">For GPU-intensive workloads, check out [NC and NV sizes](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span></span>

## <a name="samples-and-demos"></a><span data-ttu-id="4e99e-149">Exempel och demonstrationer</span><span class="sxs-lookup"><span data-stu-id="4e99e-149">Samples and demos</span></span>
* [<span data-ttu-id="4e99e-150">Azure Batch C# och Python-kodexempel</span><span class="sxs-lookup"><span data-stu-id="4e99e-150">Azure Batch C# and Python code samples</span></span>](https://github.com/Azure/azure-batch-samples)
* <span data-ttu-id="4e99e-151">[Batch-skeppsvarv](https://azure.github.io/batch-shipyard/) toolkit för att underlätta distributionen av batch-format Dockerized arbetsbelastningar tooAzure Batch</span><span class="sxs-lookup"><span data-stu-id="4e99e-151">[Batch Shipyard](https://azure.github.io/batch-shipyard/) toolkit for easy deployment of batch-style Dockerized workloads tooAzure Batch</span></span>
* <span data-ttu-id="4e99e-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) R-paket, byggda på Azure Batch</span><span class="sxs-lookup"><span data-stu-id="4e99e-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) R package, built on top of Azure Batch</span></span>
* [<span data-ttu-id="4e99e-153">Test SUSE Linux Enterprise Server för HPC</span><span class="sxs-lookup"><span data-stu-id="4e99e-153">Test drive SUSE Linux Enterprise Server for HPC</span></span>](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a><span data-ttu-id="4e99e-154">Relaterade Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="4e99e-154">Related Azure services</span></span>
* [<span data-ttu-id="4e99e-155">Data Factory</span><span class="sxs-lookup"><span data-stu-id="4e99e-155">Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)
* [<span data-ttu-id="4e99e-156">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4e99e-156">Machine Learning</span></span>](https://azure.microsoft.com/documentation/services/machine-learning/)
* [<span data-ttu-id="4e99e-157">HDInsight</span><span class="sxs-lookup"><span data-stu-id="4e99e-157">HDInsight</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="4e99e-158">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="4e99e-158">Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="4e99e-159">Skaluppsättningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="4e99e-159">Virtual Machine Scale Sets</span></span>](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)
* [<span data-ttu-id="4e99e-160">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="4e99e-160">Cloud Services</span></span>](https://azure.microsoft.com/documentation/services/cloud-services/)
* [<span data-ttu-id="4e99e-161">App Service</span><span class="sxs-lookup"><span data-stu-id="4e99e-161">App Service</span></span>](https://azure.microsoft.com/documentation/services/app-service/)
* [<span data-ttu-id="4e99e-162">Media Services</span><span class="sxs-lookup"><span data-stu-id="4e99e-162">Media Services</span></span>](https://azure.microsoft.com/documentation/services/media-services/)
* [<span data-ttu-id="4e99e-163">Funktioner</span><span class="sxs-lookup"><span data-stu-id="4e99e-163">Functions</span></span>](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a><span data-ttu-id="4e99e-164">Arkitekturritningar</span><span class="sxs-lookup"><span data-stu-id="4e99e-164">Architecture blueprints</span></span>
* <span data-ttu-id="4e99e-165">[HPC-kluster och data orchestration med hjälp av Azure Batch och Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) och [artikel](../data-factory/data-factory-data-processing-using-batch.md)</span><span class="sxs-lookup"><span data-stu-id="4e99e-165">[HPC and data orchestration using Azure Batch and Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) and [article](../data-factory/data-factory-data-processing-using-batch.md)</span></span>

## <a name="industry-solutions"></a><span data-ttu-id="4e99e-166">Branschlösningar</span><span class="sxs-lookup"><span data-stu-id="4e99e-166">Industry solutions</span></span>
* [<span data-ttu-id="4e99e-167">Banker och stora marknader</span><span class="sxs-lookup"><span data-stu-id="4e99e-167">Banking and capital markets</span></span>](https://finance.azure.com/)
* [<span data-ttu-id="4e99e-168">Tekniker simulering</span><span class="sxs-lookup"><span data-stu-id="4e99e-168">Engineering simulations</span></span>](https://simulation.azure.com/) 

## <a name="customer-stories"></a><span data-ttu-id="4e99e-169">Kundberättelser</span><span class="sxs-lookup"><span data-stu-id="4e99e-169">Customer stories</span></span>
* [<span data-ttu-id="4e99e-170">ANEO</span><span class="sxs-lookup"><span data-stu-id="4e99e-170">ANEO</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 
* [<span data-ttu-id="4e99e-171">d3View</span><span class="sxs-lookup"><span data-stu-id="4e99e-171">d3View</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)
* [<span data-ttu-id="4e99e-172">Ludwig Institute of Bröstcancerdiagnoser Research</span><span class="sxs-lookup"><span data-stu-id="4e99e-172">Ludwig Institute of Cancer Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)
* [<span data-ttu-id="4e99e-173">Microsoft Research</span><span class="sxs-lookup"><span data-stu-id="4e99e-173">Microsoft Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)
* [<span data-ttu-id="4e99e-174">Milliman</span><span class="sxs-lookup"><span data-stu-id="4e99e-174">Milliman</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)
* [<span data-ttu-id="4e99e-175">Mitsubishi: A värdepapper International</span><span class="sxs-lookup"><span data-stu-id="4e99e-175">Mitsubishi UFJ Securities International</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)
* [<span data-ttu-id="4e99e-176">Schlumberger</span><span class="sxs-lookup"><span data-stu-id="4e99e-176">Schlumberger</span></span>](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [<span data-ttu-id="4e99e-177">Watson för mobiltelefoner</span><span class="sxs-lookup"><span data-stu-id="4e99e-177">Towers Watson</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)
* [<span data-ttu-id="4e99e-178">UberCloud</span><span class="sxs-lookup"><span data-stu-id="4e99e-178">UberCloud</span></span>](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

## <a name="next-steps"></a><span data-ttu-id="4e99e-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e99e-179">Next steps</span></span>
* <span data-ttu-id="4e99e-180">Senaste hello-meddelanden finns hello [Microsoft HPC och Batch-teamets blogg](http://blogs.technet.com/b/windowshpc/) och hello [Azure blogg](https://azure.microsoft.com/blog/tag/hpc/).</span><span class="sxs-lookup"><span data-stu-id="4e99e-180">For hello latest announcements, see hello [Microsoft HPC and Batch team blog](http://blogs.technet.com/b/windowshpc/) and hello [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span></span>
* <span data-ttu-id="4e99e-181">Se även [vad är nytt i Batch](https://azure.microsoft.com/updates/?service=batch) eller prenumerera toohello [RSS-feed](https://azure.microsoft.com/updates/feed/?service=batch).</span><span class="sxs-lookup"><span data-stu-id="4e99e-181">Also see [what's new in Batch](https://azure.microsoft.com/updates/?service=batch) or subscribe toohello [RSS feed](https://azure.microsoft.com/updates/feed/?service=batch).</span></span>

