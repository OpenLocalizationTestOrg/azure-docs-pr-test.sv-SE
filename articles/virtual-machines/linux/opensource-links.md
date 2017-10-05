---
title: "Linux- och öppen källkod Computing i Azure | Microsoft Docs"
description: "Visar en lista över Linux och öppen källkod Computing artiklar på Azure, inklusive användning av grundläggande Linux vissa grundläggande begrepp för att köra eller uppladdning av Linux-avbildningar i Azure och annat innehåll om specifika tekniker och optimeringar."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: a7e608b5-26ea-41e0-b46b-1a483a257754
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/27/2016
ms.author: rasquill
ms.openlocfilehash: 1cdd0e68368d2dc376ee45df67bf5e75288d4ca3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="linux-and-open-source-computing-on-azure"></a>Linux och databearbetning med öppen källkod på Azure
Hitta all dokumentation som du behöver skapa och hantera Linux-baserade virtuella datorer i den klassiska distributionsmodellen.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../resource-manager-deployment-model.md). Den här artikeln täcker den klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.

## <a name="get-started"></a>Kom igång
* [Introduktion för Linux på Azure](intro-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vanliga frågor om Azure virtuella datorer som skapats med den klassiska distributionsmodellen](classic/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Om avbildningar för virtuella datorer](../windows/classic/about-images.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Ladda upp en egen avbildning Distro](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) (och även anvisningar som använder en [Azure-Endorsed Distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json))
* [Logga in på en Linux VM med hjälp av den klassiska Azure-portalen](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="set-up"></a>Konfigurera
* [Installera Azure kommandoradsgränssnittet (Azure CLI)](../../cli-install-nodejs.md)

## <a name="tutorials"></a>Självstudier
* [Installera LYKTA stacken på en virtuell Linux-dator i Azure](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ruby på spår webbprogram på en Azure VM](classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
* [Så här: Installera Apache Qpid Proton-C för AMQP och Service Bus](../../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Databaser
* [Optimera prestanda för MySQL på Azure](classic/optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MySQL-kluster](classic/mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kör Cassandra med Linux på Azure och komma åt den från Node.js](classic/cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Skapa ett multimaster MariaDbs-kluster](classic/mariadb-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="hpc"></a>HPC-KLUSTER
* [Kom igång med Linux compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kör NAMD med Microsoft HPC Pack på Linux compute-noder i Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Ställa in ett Linux RDMA-kluster som kör MPI-program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="docker"></a>Docker
* [Använda tillägget för virtuella datorer med Docker genom kommandoradsgränssnittet för Azure (Azure CLI)](classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Använda tillägget för virtuella datorer med Docker från Azure Portal](classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Så använder du en Docker-dator med Azure](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="ubuntu"></a>Ubuntu
* [Så här: MySQL-kluster](classic/mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Så här: Node.js och Cassandra](classic/cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="opensuse"></a>OpenSUSE
* [Så här: Installera och köra MySQL](classic/mysql-on-opensuse.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="coreos"></a>CoreOS
* [Så här: använda virtuell CoreOS på Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="planning"></a>Planering
* [Vägledning för implementering av Azure-infrastrukturtjänster](../windows/infrastructure-subscription-accounts-guidelines.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Att välja Linux-användarnamn](usernames.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Så här konfigurerar du en tillgänglighetsuppsättning för virtuella datorer i den klassiska distributionsmodellen](../windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Så här schemalägger du planerat underhåll på virtuella Azure-datorer](classic/planned-maintenance-schedule.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hantera tillgängligheten för virtuella datorer](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planerat underhåll för Linux virtuella datorer i Azure](planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="deployment"></a>Distribution
* [Skapa en egen virtuell dator som kör Linux](../windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Grunderna: samla in en Linux VM för att skapa en mall](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Information om icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="management"></a>Hantering
* [SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hur du återställer ett lösenord eller SSH-egenskaper för Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Med hjälp av rot](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="azure-resources"></a>Azure-resurser
* [Azure Linux-agenten](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VM-tillägg och funktioner](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Placera anpassade Data till en virtuell dator att använda med molnet initiering](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="storage"></a>Lagring
* [Koppla en datadisk till en virtuell Linux-dator](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kopplar från en datadisk från en virtuell Linux-dator](classic/detach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Nätverk
* [Hur du ställer in slutpunkter på en klassisk virtuell dator i Azure](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="troubleshooting"></a>Felsökning
* [Felsökning av SSH (Secure Shell) anslutningar till en Linux-baserade Azure virtuella](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Felsöka klassisk distributionsproblem med att skapa en ny virtuell Linux-dator i Azure](classic/troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)  
* [Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig virtuell Linux-dator i Azure](../windows/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) 

## <a name="reference"></a>Referens
* [Azure CLI-kommandona i Azure Service Management (asm)-läge](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)

## <a name="general-links"></a>Allmänna länkar
Följande länkar gäller Microsoft bloggar, Technet-sidor och externa webbplatser i stället Azure.com dokumentationen som ovan. Både Azure och öppen källkod datoranvändning är fast flytta mål, det är nästan säkert att följande länkar är inaktuella *trots* det faktum att vi skall gör oss att lägga till nyare ämnen och ta bort inaktuella kontinuerligt. Om vi har missat en ta Låt oss veta i kommentarerna, eller skicka en pullbegäran till vår [GitHub-repo-](https://github.com/Azure/azure-content/).

* [Kör ASP.NET 5 i Linux med Docker-behållare](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
* [Så här distribuerar du en CentOS VM-avbildning från OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
* [SUSE infrastrukturen](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
* [SUSE Linux Enterprise Server för SAP Molnbibliotek installation](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
* [Skapa hög tillgänglighet Linux i Azure i steg 12](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
* [Automatisera etablerar Linux på Azure med Azure CLI, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
* [GlusterFS på Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
* [Kör FreeBSD i Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
* [Enkelt distribuera FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
* [Distribuera en anpassad FreeBSD-avbildning](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
* [Kaspersky AV för Linux-filserver](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL
* [8 öppen källkod NoSql-databaser för Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
* [Slideshare (MSOpenTech): Upplevelser med CouchDb på Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
* [Kör CouchDB-as-a-Service med node.js, CORS och Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
* [Redis i Windows i tjänsten Azure Redis-Cache](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
* [Om ASP.NET-Sessionstillståndsprovider för Redis-förhandsversionen](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
* [Blogg: RavenHQ finns nu tillgängligt i Azure Marketplace](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Big Data
* [Installera Hadoop på virtuella Azure Linux-datorer](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
* [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relationsdatabas
* [Arkitektur för MySQL hög tillgänglighet i Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
* [Installera Postgres med corosync pg_bouncer med ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux högpresterande datorbearbetning (HPC)
* [Snabbstart mall: få igång en SLURM klustret](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (och [blogginlägget](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
* [Mall för Snabbstart: skapa ett HPC-kluster med beräkningsnoder för Linux](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops-, hanterings- och optimering
Som en värld av devops hantering och optimering är ganska expansiva och ändra mycket snabbt, bör du i listan nedan utgångspunkt.

* [Video: Virtuella Azure-datorer: med Chef, Puppet och Docker för att hantera virtuella Linux-datorer](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)
* [Fullständig handbok för automatisk Kubernetes-klusterdistribution med CoreOS och Weave](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
* [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Jenkins slavserver plugin-program för Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
* [GitHub-repo: Lagringsplugin-programmet Jenkins för Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
* [Tredje part: Det sekundära plugin-programmet Hudson för Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
* [Tredje part: Plugin-programmet Hudson för lagring på Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Video: Vad är Chef och hur fungerar det?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)
* [Video: How to Use Azure Automation with Linux VMs (Så här använder du Azure Automation med virtuella Linux-datorer)](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* [Blogg: How to do Powershell DSC for Linux (Så här använder du Powershell DSC för Linux)](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
* [GitHub: Docker-klient DSC](https://github.com/anweiss/DockerClientDSC)
* [Packare plugin-program för Azure](https://github.com/msopentech/packer-azure)

