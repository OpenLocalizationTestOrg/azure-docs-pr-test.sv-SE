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
# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a>Stora Compute i Azure: tekniska resurser för batch- och högpresterande datorbearbetning
Det här är en guide tootechnical resurser toohelp som du kör din storskaliga parallellt, batch och högpresterande datorbearbetning (HPC) arbetsbelastningar i Azure. Utöka din befintliga batch eller HPC arbetsbelastningar toohello Azure-molnet eller skapa nya stort Compute-lösningar med hjälp av ett intervall av Azure-tjänster.

## <a name="solutions-options"></a>Alternativ för lösningar
Lär dig mer om stora Compute alternativ i Azure och välja hello rätt metod för din arbetsbelastning och företag behöver.

* [Batch- och HPC-lösningar](batch-hpc-solutions.md)
* [Video: Stor beräkning i hello moln med Azure och HPC](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)

## <a name="azure-batch"></a>Azure Batch
[Batch](https://azure.microsoft.com/services/batch/) är en platform-tjänst som gör det enkelt toocloud – aktivera Linux och Windows-program och kör jobb utan att ställa in och hantera ett kluster och jobbet Schemaläggaren. Använder hello SDK toointegrate program med Azure Batch via olika språk, mellanlagra data tooAzure och skapa pipelines för körning av jobbet.

* [Dokumentation](https://azure.microsoft.com/documentation/services/batch/)
* [.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), och [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API-referens
* [Batch management .NET-bibliotek](https://msdn.microsoft.com/library/mt463120.aspx) referens
* Självstudier: Kom igång med [Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md) och [Batch Python-klient](batch-python-tutorial.md)
* [Batch-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)
* [Batch-videor](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a>HPC-kluster-lösningar
Distribuera eller utöka din befintliga Windows eller Linux HPC-kluster tooAzure toorun beräknings-arbetsbelastningar.  

### <a name="microsoft-hpc-pack"></a>Microsoft HPC Pack
HPC Pack är Microsofts ledigt HPC-lösning baserat på Microsoft Azure och Windows Server-teknik kan köra Windows och Linux HPC arbetsbelastningar.  

* [Hämta HPC Pack 2016](https://www.microsoft.com/download/details.aspx?id=54507)
* [Hämta HPC Pack 2012 R2 uppdatering 3](https://www.microsoft.com/download/details.aspx?id=49922)
* [Dokumentation](https://technet.microsoft.com/library/jj899572.aspx)
* HPC Pack klustret alternativ i Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 
* [Burst tooAzure worker-instanser med HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)
* [TooAzure burst med HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)
* [Windows HPC-forum](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a>Lösningar för Linux och OSS
Använd dessa mallar för Azure-toodeploy Linux HPC-kluster.

* [Få igång en SLURM klustret](https://azure.microsoft.com/documentation/templates/slurm/) och [blogginlägget](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)
* [Få igång ett moment kluster](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [Beräkna rutnätet mallar med PBS Professional](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

## <a name="hpc-storage"></a>HPC-lagring
* [Parallell filsystem för HPC-lagring på Azure](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)
* [Intel molnet Edition för Lyster programvara - utvärdering](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)
* [BeeGFS på CentOS 7.2 mall](https://github.com/smith1511/hpc/tree/master/beegfs-shared-on-centos7.2)




## <a name="microsoft-mpi"></a>Microsoft MPI
[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) är en Microsoft-implementering av hello Message Passing Interface standard för att utveckla och parallella program som körs på Windows hello-plattformen.

* [Ladda ned MS-MPI](http://go.microsoft.com/FWLink/p/?LinkID=389556)
* [MS-MPI-referens](https://msdn.microsoft.com/library/dn473458.aspx)
* [MPI-forum](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a>Beräkningsintensiva instanser
Azure erbjuder en [intervall för VM-storlekar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), inklusive [beräkningsintensiva H-serien](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instanser kan ansluta tooa RDMA serverdelsnätverk, toorun din Linux och Windows HPC-arbetsbelastning. 

* [Ställa in en Linux RDMA klustret toorun MPI-program](../virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Konfigurera ett RDMA för Windows-kluster med Microsoft HPC Pack toorun MPI program](../virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

För GPU-intensiva arbetsbelastningar, kolla [NC-och NV](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).

## <a name="samples-and-demos"></a>Exempel och demonstrationer
* [Azure Batch C# och Python-kodexempel](https://github.com/Azure/azure-batch-samples)
* [Batch-skeppsvarv](https://azure.github.io/batch-shipyard/) toolkit för att underlätta distributionen av batch-format Dockerized arbetsbelastningar tooAzure Batch
* [doAzureParallel](http://www.github.com/Azure/doAzureParallel) R-paket, byggda på Azure Batch
* [Test SUSE Linux Enterprise Server för HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a>Relaterade Azure-tjänster
* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Skaluppsättningar för den virtuella datorn](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)
* [Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)
* [App Service](https://azure.microsoft.com/documentation/services/app-service/)
* [Media Services](https://azure.microsoft.com/documentation/services/media-services/)
* [Funktioner](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a>Arkitekturritningar
* [HPC-kluster och data orchestration med hjälp av Azure Batch och Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) och [artikel](../data-factory/data-factory-data-processing-using-batch.md)

## <a name="industry-solutions"></a>Branschlösningar
* [Banker och stora marknader](https://finance.azure.com/)
* [Tekniker simulering](https://simulation.azure.com/) 

## <a name="customer-stories"></a>Kundberättelser
* [ANEO](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 
* [d3View](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)
* [Ludwig Institute of Bröstcancerdiagnoser Research](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)
* [Microsoft Research](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)
* [Milliman](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)
* [Mitsubishi: A värdepapper International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Watson för mobiltelefoner](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)
* [UberCloud](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

## <a name="next-steps"></a>Nästa steg
* Senaste hello-meddelanden finns hello [Microsoft HPC och Batch-teamets blogg](http://blogs.technet.com/b/windowshpc/) och hello [Azure blogg](https://azure.microsoft.com/blog/tag/hpc/).
* Se även [vad är nytt i Batch](https://azure.microsoft.com/updates/?service=batch) eller prenumerera toohello [RSS-feed](https://azure.microsoft.com/updates/feed/?service=batch).

