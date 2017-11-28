---
title: "aaaClusterize MySQL med belastningsutjämnade uppsättningar | Microsoft Docs"
description: "Konfigurera en belastningsutjämnad hög tillgänglighet Linux MySQL-kluster skapas med hello klassiska distributionsmodellen på Azure"
services: virtual-machines-linux
documentationcenter: 
author: bureado
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: 1829fd877c4b0ed177b23a8e3404dbb3db746561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-load-balanced-sets-tooclusterize-mysql-on-linux"></a><span data-ttu-id="f2b10-103">Använda belastningsutjämnade uppsättningar tooclusterize MySQL på Linux</span><span class="sxs-lookup"><span data-stu-id="f2b10-103">Use load-balanced sets tooclusterize MySQL on Linux</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f2b10-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk.</span><span class="sxs-lookup"><span data-stu-id="f2b10-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="f2b10-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f2b10-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="f2b10-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="f2b10-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="f2b10-107">En [Resource Manager-mall](https://azure.microsoft.com/documentation/templates/mysql-replication/) är tillgänglig om du behöver toodeploy MySQL-kluster.</span><span class="sxs-lookup"><span data-stu-id="f2b10-107">A [Resource Manager template](https://azure.microsoft.com/documentation/templates/mysql-replication/) is available if you need toodeploy a MySQL cluster.</span></span>

<span data-ttu-id="f2b10-108">Den här artikeln utforskar och visar hello olika metoder tillgängliga toodeploy Linux-baserade tjänster med hög tillgänglighet i Microsoft Azure, utforska MySQL hög tillgänglighet som en introduktion.</span><span class="sxs-lookup"><span data-stu-id="f2b10-108">This article explores and illustrates hello different approaches available toodeploy highly available Linux-based services on Microsoft Azure, exploring MySQL Server high availability as a primer.</span></span> <span data-ttu-id="f2b10-109">En video som visar den här metoden är tillgängligt på [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span><span class="sxs-lookup"><span data-stu-id="f2b10-109">A video illustrating this approach is available on [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).</span></span>

<span data-ttu-id="f2b10-110">Vi kommer disposition delat ingenting, två noder, single master MySQL lösning för hög tillgänglighet baserat på DRBD, Corosync och Pacemaker.</span><span class="sxs-lookup"><span data-stu-id="f2b10-110">We will outline a shared-nothing, two-node, single-master MySQL high availability solution based on DRBD, Corosync, and Pacemaker.</span></span> <span data-ttu-id="f2b10-111">Endast en nod kör MySQL i taget.</span><span class="sxs-lookup"><span data-stu-id="f2b10-111">Only one node runs MySQL at a time.</span></span> <span data-ttu-id="f2b10-112">Läsning och skrivning från hello DRBD resursen är också begränsad tooonly en nod i taget.</span><span class="sxs-lookup"><span data-stu-id="f2b10-112">Reading and writing from hello DRBD resource is also limited tooonly one node at a time.</span></span>

<span data-ttu-id="f2b10-113">Det finns inget behov av en VIP-lösning som LVS, eftersom du ska använda belastningsutjämnade uppsättningar i Microsoft Azure tooprovide resursallokering funktioner och slutpunkten identifiering, borttagning och korrekt återställning av hello VIP.</span><span class="sxs-lookup"><span data-stu-id="f2b10-113">There's no need for a VIP solution like LVS, because you'll use load-balanced sets in Microsoft Azure tooprovide round-robin functionality and endpoint detection, removal, and graceful recovery of hello VIP.</span></span> <span data-ttu-id="f2b10-114">hello VIP är en globalt dirigeras IPv4-adress som tilldelats av Microsoft Azure när du skapar hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2b10-114">hello VIP is a globally routable IPv4 address assigned by Microsoft Azure when you first create hello cloud service.</span></span>

<span data-ttu-id="f2b10-115">Det finns andra möjliga arkitekturer för MySQL, inklusive nästa arbetsdag kluster, Percona, Galera och flera mellanprogram lösningar, inklusive minst en tillgänglig som en virtuell dator på [VM anläggningen](http://vmdepot.msopentech.com).</span><span class="sxs-lookup"><span data-stu-id="f2b10-115">There are other possible architectures for MySQL, including NBD Cluster, Percona, Galera, and several middleware solutions, including at least one available as a VM on [VM Depot](http://vmdepot.msopentech.com).</span></span> <span data-ttu-id="f2b10-116">Så länge dessa lösningar kan replikera på unicast kontra multicast eller broadcast inte och på delad lagring eller flera nätverksgränssnitt, bör hello scenarier vara enkelt toodeploy på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f2b10-116">As long as these solutions can replicate on unicast vs. multicast or broadcast and don't rely on shared storage or multiple network interfaces, hello scenarios should be easy toodeploy on Microsoft Azure.</span></span>

<span data-ttu-id="f2b10-117">Dessa kluster arkitekturer kan utökas tooother produkter som PostgreSQL och OpenLDAP på ett liknande sätt.</span><span class="sxs-lookup"><span data-stu-id="f2b10-117">These clustering architectures can be extended tooother products like PostgreSQL and OpenLDAP in a similar fashion.</span></span> <span data-ttu-id="f2b10-118">Till exempel belastningsutjämning proceduren med delat ingenting har har testats med flera master OpenLDAP och du kan titta på den på vår Channel 9-bloggen.</span><span class="sxs-lookup"><span data-stu-id="f2b10-118">For example, this load-balancing procedure with shared nothing was successfully tested with multi-master OpenLDAP, and you can watch it on our Channel 9 blog.</span></span>

## <a name="get-ready"></a><span data-ttu-id="f2b10-119">Gör dig redo</span><span class="sxs-lookup"><span data-stu-id="f2b10-119">Get ready</span></span>
<span data-ttu-id="f2b10-120">Du behöver följande hello resurser och funktioner:</span><span class="sxs-lookup"><span data-stu-id="f2b10-120">You need hello following resources and abilities:</span></span>

  - <span data-ttu-id="f2b10-121">En Microsoft Azure-konto med en giltig prenumeration kan toocreate minst två virtuella datorer (XS användes i det här exemplet)</span><span class="sxs-lookup"><span data-stu-id="f2b10-121">A Microsoft Azure account with a valid subscription, able toocreate at least two VMs (XS was used in this example)</span></span>
  - <span data-ttu-id="f2b10-122">Ett nätverk och ett undernät</span><span class="sxs-lookup"><span data-stu-id="f2b10-122">A network and a subnet</span></span>
  - <span data-ttu-id="f2b10-123">En tillhörighetsgrupp</span><span class="sxs-lookup"><span data-stu-id="f2b10-123">An affinity group</span></span>
  - <span data-ttu-id="f2b10-124">En tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="f2b10-124">An availability set</span></span>
  - <span data-ttu-id="f2b10-125">Hej möjlighet toocreate virtuella hårddiskar i hello samma region som hello tjänst i molnet och koppla dem toohello virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="f2b10-125">hello ability toocreate VHDs in hello same region as hello cloud service and attach them toohello Linux VMs</span></span>

### <a name="tested-environment"></a><span data-ttu-id="f2b10-126">Testad miljö</span><span class="sxs-lookup"><span data-stu-id="f2b10-126">Tested environment</span></span>
* <span data-ttu-id="f2b10-127">Ubuntu 13.10</span><span class="sxs-lookup"><span data-stu-id="f2b10-127">Ubuntu 13.10</span></span>
  * <span data-ttu-id="f2b10-128">DRBD</span><span class="sxs-lookup"><span data-stu-id="f2b10-128">DRBD</span></span>
  * <span data-ttu-id="f2b10-129">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="f2b10-129">MySQL Server</span></span>
  * <span data-ttu-id="f2b10-130">Corosync och Pacemaker</span><span class="sxs-lookup"><span data-stu-id="f2b10-130">Corosync and Pacemaker</span></span>

### <a name="affinity-group"></a><span data-ttu-id="f2b10-131">Tillhörighetsgruppen</span><span class="sxs-lookup"><span data-stu-id="f2b10-131">Affinity group</span></span>
<span data-ttu-id="f2b10-132">Skapa en tillhörighetsgrupp för hello lösningen genom att logga in toohello klassiska Azure-portalen att välja **inställningar**, och skapa en tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="f2b10-132">Create an affinity group for hello solution by signing in toohello Azure classic portal, selecting **Settings**, and creating an affinity group.</span></span> <span data-ttu-id="f2b10-133">Resurserna som skapas senare kommer att tilldelas toothis tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="f2b10-133">Allocated resources created later will be assigned toothis affinity group.</span></span>

### <a name="networks"></a><span data-ttu-id="f2b10-134">Nätverk</span><span class="sxs-lookup"><span data-stu-id="f2b10-134">Networks</span></span>
<span data-ttu-id="f2b10-135">Ett nytt nätverk skapas och ett undernät som har skapats i hello-nätverk.</span><span class="sxs-lookup"><span data-stu-id="f2b10-135">A new network is created, and a subnet is created inside hello network.</span></span> <span data-ttu-id="f2b10-136">Det här exemplet använder ett 10.10.10.0/24 nätverk med endast en /24 undernät i.</span><span class="sxs-lookup"><span data-stu-id="f2b10-136">This example uses a 10.10.10.0/24 network with only one /24 subnet inside.</span></span>

### <a name="virtual-machines"></a><span data-ttu-id="f2b10-137">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f2b10-137">Virtual machines</span></span>
<span data-ttu-id="f2b10-138">hello första Ubuntu 13.10 VM skapas med hjälp av en bild Endorsed Ubuntu galleriet och kallas `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-138">hello first Ubuntu 13.10 VM is created by using an Endorsed Ubuntu Gallery image and is called `hadb01`.</span></span> <span data-ttu-id="f2b10-139">En ny molntjänst skapas i hello process kallas hadb.</span><span class="sxs-lookup"><span data-stu-id="f2b10-139">A new cloud service is created in hello process, called hadb.</span></span> <span data-ttu-id="f2b10-140">Det här namnet visas delade hello belastningsutjämnade natur hello-tjänsten har när du lägger till fler resurser.</span><span class="sxs-lookup"><span data-stu-id="f2b10-140">This name illustrates hello shared, load-balanced nature that hello service will have when more resources are added.</span></span> <span data-ttu-id="f2b10-141">Hej skapandet av `hadb01` är primärdomänkontrollant och slutförda med hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f2b10-141">hello creation of `hadb01` is uneventful and completed by using hello portal.</span></span> <span data-ttu-id="f2b10-142">En slutpunkt för SSH skapas automatiskt och hello nytt nätverk har valts.</span><span class="sxs-lookup"><span data-stu-id="f2b10-142">An endpoint for SSH is automatically created, and hello new network is selected.</span></span> <span data-ttu-id="f2b10-143">Nu kan du skapa en tillgänglighetsuppsättning för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f2b10-143">Now you can create an availability set for hello VMs.</span></span>

<span data-ttu-id="f2b10-144">När hello första virtuella datorn skapas (tekniskt sett när hello Molntjänsten skapas), skapa hello andra VM `hadb02`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-144">After hello first VM is created (technically, when hello cloud service is created), create hello second VM, `hadb02`.</span></span> <span data-ttu-id="f2b10-145">Hello andra i VM, använda Ubuntu 13.10 VM från hello galleriet med hello-portalen, men en befintlig molntjänst `hadb.cloudapp.net`, i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="f2b10-145">For hello second VM, use Ubuntu 13.10 VM from hello Gallery by using hello portal, but use an existing cloud service, `hadb.cloudapp.net`, instead of creating a new one.</span></span> <span data-ttu-id="f2b10-146">hello nätverk och tillgänglighet som ska väljas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f2b10-146">hello network and availability set should be automatically selected.</span></span> <span data-ttu-id="f2b10-147">En SSH-slutpunkt skapas för.</span><span class="sxs-lookup"><span data-stu-id="f2b10-147">An SSH endpoint will be created, too.</span></span>

<span data-ttu-id="f2b10-148">När båda VM: ar har skapats, anteckna hello SSH-port för `hadb01` (TCP 22) och `hadb02` (tilldelade automatiskt av Azure).</span><span class="sxs-lookup"><span data-stu-id="f2b10-148">After both VMs have been created, take note of hello SSH port for `hadb01` (TCP 22) and `hadb02` (automatically assigned by Azure).</span></span>

### <a name="attached-storage"></a><span data-ttu-id="f2b10-149">Direktansluten lagring</span><span class="sxs-lookup"><span data-stu-id="f2b10-149">Attached storage</span></span>
<span data-ttu-id="f2b10-150">Koppla en ny disk tooboth virtuella datorer och skapa 5 GB-diskar i hello process.</span><span class="sxs-lookup"><span data-stu-id="f2b10-150">Attach a new disk tooboth VMs and create 5-GB disks in hello process.</span></span> <span data-ttu-id="f2b10-151">hello diskar finns i hello VHD-behållare för huvudsakliga operativsystem-diskar.</span><span class="sxs-lookup"><span data-stu-id="f2b10-151">hello disks are hosted in hello VHD container in use for your main operating system disks.</span></span> <span data-ttu-id="f2b10-152">När diskar har skapats och ansluten, finns inga behov toorestart Linux eftersom hello kernel visas hello ny enhet.</span><span class="sxs-lookup"><span data-stu-id="f2b10-152">After disks are created and attached, there is no need toorestart Linux because hello kernel will see hello new device.</span></span> <span data-ttu-id="f2b10-153">Den här enheten är vanligtvis `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-153">This device is usually `/dev/sdc`.</span></span> <span data-ttu-id="f2b10-154">Kontrollera `dmesg` för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="f2b10-154">Check `dmesg` for hello output.</span></span>

<span data-ttu-id="f2b10-155">Skapa en partition på varje virtuell dator med hjälp av `cfdisk` (primära Linux partition) och skriva hello nya partitionstabell.</span><span class="sxs-lookup"><span data-stu-id="f2b10-155">On each VM, create a partition by using `cfdisk` (primary, Linux partition) and write hello new partition table.</span></span> <span data-ttu-id="f2b10-156">Skapa ett filsystem på den här partitionen.</span><span class="sxs-lookup"><span data-stu-id="f2b10-156">Do not create a file system on this partition.</span></span>

## <a name="set-up-hello-cluster"></a><span data-ttu-id="f2b10-157">Konfigurera hello kluster</span><span class="sxs-lookup"><span data-stu-id="f2b10-157">Set up hello cluster</span></span>
<span data-ttu-id="f2b10-158">Använd LGH tooinstall Corosync och Pacemaker DRBD på båda Ubuntu VM: ar.</span><span class="sxs-lookup"><span data-stu-id="f2b10-158">Use APT tooinstall Corosync, Pacemaker, and DRBD on both Ubuntu VMs.</span></span> <span data-ttu-id="f2b10-159">toodo så med `apt-get`kör hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="f2b10-159">toodo so with `apt-get`, run hello following code:</span></span>

    sudo apt-get install corosync pacemaker drbd8-utils.

<span data-ttu-id="f2b10-160">Installera inte MySQL just nu.</span><span class="sxs-lookup"><span data-stu-id="f2b10-160">Do not install MySQL at this time.</span></span> <span data-ttu-id="f2b10-161">Debian och Ubuntu installationsskript initiera en MySQL-datakatalog på `/var/lib/mysql`, men eftersom hello directory ska ersättas av ett DRBD filsystem, måste tooinstall MySQL senare.</span><span class="sxs-lookup"><span data-stu-id="f2b10-161">Debian and Ubuntu installation scripts will initialize a MySQL data directory on `/var/lib/mysql`, but because hello directory will be superseded by a DRBD file system, you need tooinstall MySQL later.</span></span>

<span data-ttu-id="f2b10-162">Kontrollera (med hjälp av `/sbin/ifconfig`) att både virtuella datorer använder adresser i hello 10.10.10.0/24 undernät och att de kan pinga varandra efter namn.</span><span class="sxs-lookup"><span data-stu-id="f2b10-162">Verify (by using `/sbin/ifconfig`) that both VMs are using addresses in hello 10.10.10.0/24 subnet and that they can ping each other by name.</span></span> <span data-ttu-id="f2b10-163">Du kan också använda `ssh-keygen` och `ssh-copy-id` toomake att båda VM: ar kan kommunicera via SSH utan att kräva ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="f2b10-163">You can also use `ssh-keygen` and `ssh-copy-id` toomake sure both VMs can communicate via SSH without requiring a password.</span></span>

### <a name="set-up-drbd"></a><span data-ttu-id="f2b10-164">Ställ in DRBD</span><span class="sxs-lookup"><span data-stu-id="f2b10-164">Set up DRBD</span></span>
<span data-ttu-id="f2b10-165">Skapa en resurs för DRBD som använder hello underliggande `/dev/sdc1` partitions tooproduce en `/dev/drbd1` resurs som formaterats med ext3 och används i både primära och sekundära noder.</span><span class="sxs-lookup"><span data-stu-id="f2b10-165">Create a DRBD resource that uses hello underlying `/dev/sdc1` partition tooproduce a `/dev/drbd1` resource that can be formatted by using ext3 and used in both primary and secondary nodes.</span></span>

1. <span data-ttu-id="f2b10-166">Öppna `/etc/drbd.d/r0.res` och kopiera hello följa resursdefinitionen på båda VM: ar:</span><span class="sxs-lookup"><span data-stu-id="f2b10-166">Open `/etc/drbd.d/r0.res` and copy hello following resource definition on both VMs:</span></span>

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. <span data-ttu-id="f2b10-167">Initiera hello resursen med hjälp av `drbdadm` på både virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="f2b10-167">Initialize hello resource by using `drbdadm` on both VMs:</span></span>

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. <span data-ttu-id="f2b10-168">På hello primära virtuella datorn (`hadb01`), force ägarskapet (primär) för hello DRBD resurs:</span><span class="sxs-lookup"><span data-stu-id="f2b10-168">On hello primary VM (`hadb01`), force ownership (primary) of hello DRBD resource:</span></span>

        sudo drbdadm primary --force r0

<span data-ttu-id="f2b10-169">Om du undersöker hello innehållet i proc/drbd (`sudo cat /proc/drbd`) på båda VM: ar, bör du se `Primary/Secondary` på `hadb01` och `Secondary/Primary` på `hadb02`och konsekvent med nu hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="f2b10-169">If you examine hello contents of /proc/drbd (`sudo cat /proc/drbd`) on both VMs, you should see `Primary/Secondary` on `hadb01` and `Secondary/Primary` on `hadb02`, consistent with hello solution at this point.</span></span> <span data-ttu-id="f2b10-170">hello 5 GB disk är synkroniserad över hello 10.10.10.0/24 nätverk på toocustomers utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="f2b10-170">hello 5-GB disk is synchronized over hello 10.10.10.0/24 network at no charge toocustomers.</span></span>

<span data-ttu-id="f2b10-171">När hello disk synkroniseras, kan du skapa hello filsystemet på `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-171">After hello disk is synchronized, you can create hello file system on `hadb01`.</span></span> <span data-ttu-id="f2b10-172">I testsyfte kan vi använde ext2 men hello följande kod skapar ett ext3 filsystem:</span><span class="sxs-lookup"><span data-stu-id="f2b10-172">For testing purposes, we used ext2, but hello following code will create an ext3 file system:</span></span>

    mkfs.ext3 /dev/drbd1

### <a name="mount-hello-drbd-resource"></a><span data-ttu-id="f2b10-173">Montera hello DRBD resurs</span><span class="sxs-lookup"><span data-stu-id="f2b10-173">Mount hello DRBD resource</span></span>
<span data-ttu-id="f2b10-174">Du är nu redo toomount hello DRBD resurser på `hadb01`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-174">You're now ready toomount hello DRBD resources on `hadb01`.</span></span> <span data-ttu-id="f2b10-175">Debian och derivat Använd `/var/lib/mysql` som Mysqls datakatalog.</span><span class="sxs-lookup"><span data-stu-id="f2b10-175">Debian and derivatives use `/var/lib/mysql` as MySQL's data directory.</span></span> <span data-ttu-id="f2b10-176">Eftersom du inte har installerat MySQL skapa hello katalog och montera hello DRBD resurs.</span><span class="sxs-lookup"><span data-stu-id="f2b10-176">Because you haven't installed MySQL, create hello directory and mount hello DRBD resource.</span></span> <span data-ttu-id="f2b10-177">tooperform det här alternativet Kör hello följande kod `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="f2b10-177">tooperform this option, run hello following code on `hadb01`:</span></span>

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a><span data-ttu-id="f2b10-178">Ställ in MySQL</span><span class="sxs-lookup"><span data-stu-id="f2b10-178">Set up MySQL</span></span>
<span data-ttu-id="f2b10-179">Nu är du redo tooinstall MySQL på `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="f2b10-179">Now you're ready tooinstall MySQL on `hadb01`:</span></span>

    sudo apt-get install mysql-server

<span data-ttu-id="f2b10-180">För `hadb02`, har du två alternativ.</span><span class="sxs-lookup"><span data-stu-id="f2b10-180">For `hadb02`, you have two options.</span></span> <span data-ttu-id="f2b10-181">Du kan installera mysql-servern, vilket skapar /var/lib/mysql, Fyll den med en ny datakatalog, och ta sedan bort hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="f2b10-181">You can install mysql-server, which will create /var/lib/mysql, fill it with a new data directory, and then remove hello contents.</span></span> <span data-ttu-id="f2b10-182">tooperform det här alternativet Kör hello följande kod `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="f2b10-182">tooperform this option, run hello following code on `hadb02`:</span></span>

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

<span data-ttu-id="f2b10-183">hello andra alternativet är toofailover för`hadb02` och installera sedan mysql-server det.</span><span class="sxs-lookup"><span data-stu-id="f2b10-183">hello second option is toofailover too`hadb02` and then install mysql-server there.</span></span> <span data-ttu-id="f2b10-184">Installationsskript ser hello befintlig installation och påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="f2b10-184">Installation scripts will notice hello existing installation and won't touch it.</span></span>

<span data-ttu-id="f2b10-185">Kör hello följande kod i `hadb01`:</span><span class="sxs-lookup"><span data-stu-id="f2b10-185">Run hello following code on `hadb01`:</span></span>

    sudo drbdadm secondary –force r0

<span data-ttu-id="f2b10-186">Kör hello följande kod i `hadb02`:</span><span class="sxs-lookup"><span data-stu-id="f2b10-186">Run hello following code on `hadb02`:</span></span>

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

<span data-ttu-id="f2b10-187">Om du inte planerar toofailover DRBD nu, är hello första alternativet enklare även om tvekan mindre elegant.</span><span class="sxs-lookup"><span data-stu-id="f2b10-187">If you don't plan toofailover DRBD now, hello first option is easier although arguably less elegant.</span></span> <span data-ttu-id="f2b10-188">När du har du skapat kan börja du arbeta på MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="f2b10-188">After you set this up, you can start working on your MySQL database.</span></span> <span data-ttu-id="f2b10-189">Kör hello följande kod i `hadb02` (eller vilken hello servrar är aktiv, enligt tooDRBD):</span><span class="sxs-lookup"><span data-stu-id="f2b10-189">Run hello following code on `hadb02` (or whichever one of hello servers is active, according tooDRBD):</span></span>

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* tooroot;

> [!WARNING]
> <span data-ttu-id="f2b10-190">Det här sista instruktionen inaktiverar autentisering för hello rotanvändaren i den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="f2b10-190">This last statement effectively disables authentication for hello root user in this table.</span></span> <span data-ttu-id="f2b10-191">Detta bör ersättas med din produktion klass ge instruktioner och ingår endast som illustration.</span><span class="sxs-lookup"><span data-stu-id="f2b10-191">This should be replaced by your production-grade GRANT statements and is included only for illustrative purposes.</span></span>

<span data-ttu-id="f2b10-192">Om du vill toomake frågor från utanför hello virtuella datorer (som är hello syftet med den här guiden), måste du också tooenable nätverksfunktioner för MySQL.</span><span class="sxs-lookup"><span data-stu-id="f2b10-192">If you want toomake queries from outside hello VMs (which is hello purpose of this guide), you also need tooenable networking for MySQL.</span></span> <span data-ttu-id="f2b10-193">Öppna på båda VM: ar, `/etc/mysql/my.cnf` och gå för`bind-address`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-193">On both VMs, open `/etc/mysql/my.cnf` and go too`bind-address`.</span></span> <span data-ttu-id="f2b10-194">Ändra hello-adressen från 127.0.0.1 too0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="f2b10-194">Change hello address from 127.0.0.1 too0.0.0.0.</span></span> <span data-ttu-id="f2b10-195">När du har sparat filen hello utfärda en `sudo service mysql restart` på din aktuella primära.</span><span class="sxs-lookup"><span data-stu-id="f2b10-195">After saving hello file, issue a `sudo service mysql restart` on your current primary.</span></span>

### <a name="create-hello-mysql-load-balanced-set"></a><span data-ttu-id="f2b10-196">Skapa hello MySQL belastningsutjämnad uppsättning</span><span class="sxs-lookup"><span data-stu-id="f2b10-196">Create hello MySQL load-balanced set</span></span>
<span data-ttu-id="f2b10-197">Gå tillbaka toohello portal, gå för`hadb01`, och välj **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="f2b10-197">Go back toohello portal, go too`hadb01`, and choose **Endpoints**.</span></span> <span data-ttu-id="f2b10-198">toocreate en slutpunkt, välj MySQL (TCP 3306) hello listrutan och välj **skapa nya belastningsutjämnade uppsättningen**.</span><span class="sxs-lookup"><span data-stu-id="f2b10-198">toocreate an endpoint, choose MySQL (TCP 3306) from hello drop-down list and select **Create new load balanced set**.</span></span> <span data-ttu-id="f2b10-199">Namnet hello belastningsutjämnade endpoint `lb-mysql`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-199">Name hello load-balanced endpoint `lb-mysql`.</span></span> <span data-ttu-id="f2b10-200">Ange **tid** too5 sekunder minsta.</span><span class="sxs-lookup"><span data-stu-id="f2b10-200">Set **Time** too5 seconds, minimum.</span></span>

<span data-ttu-id="f2b10-201">När du har skapat hello endpoint gå för`hadb02`, Välj **slutpunkter**, och skapa en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f2b10-201">After you create hello endpoint, go too`hadb02`, choose **Endpoints**, and create an endpoint.</span></span> <span data-ttu-id="f2b10-202">Välj `lb-mysql`, och välj sedan MySQL hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="f2b10-202">Choose `lb-mysql`, and then select MySQL from hello drop-down list.</span></span> <span data-ttu-id="f2b10-203">Du kan också använda hello Azure CLI för det här steget.</span><span class="sxs-lookup"><span data-stu-id="f2b10-203">You can also use hello Azure CLI for this step.</span></span>

<span data-ttu-id="f2b10-204">Nu har du allt du behöver för manuell åtgärd hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="f2b10-204">You now have everything you need for manual operation of hello cluster.</span></span>

### <a name="test-hello-load-balanced-set"></a><span data-ttu-id="f2b10-205">Testa hello belastningsutjämnad uppsättning</span><span class="sxs-lookup"><span data-stu-id="f2b10-205">Test hello load-balanced set</span></span>
<span data-ttu-id="f2b10-206">Testerna kan utföras från en extern dator med hjälp av en MySQL-klient eller med hjälp av vissa program som körs som en Azure-webbplats phpMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="f2b10-206">Tests can be performed from an outside machine by using any MySQL client, or by using certain applications, like phpMyAdmin running as an Azure website.</span></span> <span data-ttu-id="f2b10-207">I så fall måste använt du Mysqls kommandoradsverktyget på en annan Linux ruta:</span><span class="sxs-lookup"><span data-stu-id="f2b10-207">In this case, you used MySQL's command-line tool on another Linux box:</span></span>

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a><span data-ttu-id="f2b10-208">Manuellt växling</span><span class="sxs-lookup"><span data-stu-id="f2b10-208">Manually failing over</span></span>
<span data-ttu-id="f2b10-209">Du kan simulera redundans genom att stänga av MySQL, växla DRBDS primära och starta MySQL igen.</span><span class="sxs-lookup"><span data-stu-id="f2b10-209">You can simulate failovers by shutting down MySQL, switching DRBD's primary, and starting MySQL again.</span></span>

<span data-ttu-id="f2b10-210">tooperform den här uppgiften kör hello följande kod på hadb01:</span><span class="sxs-lookup"><span data-stu-id="f2b10-210">tooperform this task, run hello following code on hadb01:</span></span>

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

<span data-ttu-id="f2b10-211">Klicka på hadb02:</span><span class="sxs-lookup"><span data-stu-id="f2b10-211">Then, on hadb02:</span></span>

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

<span data-ttu-id="f2b10-212">När du manuellt växla över du kan upprepa remote frågan och bör fungerar perfekt.</span><span class="sxs-lookup"><span data-stu-id="f2b10-212">After you fail over manually, you can repeat your remote query and it should work perfectly.</span></span>

## <a name="set-up-corosync"></a><span data-ttu-id="f2b10-213">Ställ in Corosync</span><span class="sxs-lookup"><span data-stu-id="f2b10-213">Set up Corosync</span></span>
<span data-ttu-id="f2b10-214">Corosync är hello underliggande klustret infrastruktur som krävs för Pacemaker toowork.</span><span class="sxs-lookup"><span data-stu-id="f2b10-214">Corosync is hello underlying cluster infrastructure required for Pacemaker toowork.</span></span> <span data-ttu-id="f2b10-215">Pulsslag (och andra metoder som Ultramonkey) är Corosync en delning av hello CRM-funktioner, medan Pacemaker är mer liknande tooHeartbeat i funktionalitet.</span><span class="sxs-lookup"><span data-stu-id="f2b10-215">For Heartbeat (and other methodologies like Ultramonkey), Corosync is a split of hello CRM functionalities, while Pacemaker remains more similar tooHeartbeat in functionality.</span></span>

<span data-ttu-id="f2b10-216">hello huvudsakliga begränsningen för Corosync i Azure är att Corosync föredrar multicast över broadcast över unicast-kommunikation, men Microsoft Azure-nätverk stöder endast unicast.</span><span class="sxs-lookup"><span data-stu-id="f2b10-216">hello main constraint for Corosync on Azure is that Corosync prefers multicast over broadcast over unicast communications, but Microsoft Azure networking only supports unicast.</span></span>

<span data-ttu-id="f2b10-217">Lyckligtvis har Corosync en fungerande unicast-läge.</span><span class="sxs-lookup"><span data-stu-id="f2b10-217">Fortunately, Corosync has a working unicast mode.</span></span> <span data-ttu-id="f2b10-218">hello endast verkliga begränsning är eftersom noderna inte kan kommunicera med varandra, måste toodefine hello noder i konfigurationsfilerna, inklusive deras IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="f2b10-218">hello only real constraint is that because all nodes are not communicating among themselves, you need toodefine hello nodes in your configuration files, including their IP addresses.</span></span> <span data-ttu-id="f2b10-219">Vi kan använda hello Corosync exempel filer för unicast- och ändrar binda adress, nod listor och loggning kataloger (Ubuntu använder `/var/log/corosync` medan hello exempel filer Använd `/var/log/cluster`), och aktivera kvorum verktyg.</span><span class="sxs-lookup"><span data-stu-id="f2b10-219">We can use hello Corosync example files for Unicast and change bind address, node lists, and logging directories (Ubuntu uses `/var/log/corosync` while hello example files use `/var/log/cluster`), and enable quorum tools.</span></span>

> [!NOTE]
> <span data-ttu-id="f2b10-220">Använder följande hello `transport: udpu` direktiv och hello manuellt definierade IP-adresser för båda noderna.</span><span class="sxs-lookup"><span data-stu-id="f2b10-220">Use hello following `transport: udpu` directive and hello manually defined IP addresses for both nodes.</span></span>

<span data-ttu-id="f2b10-221">Kör hello följande kod i `/etc/corosync/corosync.conf` för båda noderna:</span><span class="sxs-lookup"><span data-stu-id="f2b10-221">Run hello following code on `/etc/corosync/corosync.conf` for both nodes:</span></span>

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

<span data-ttu-id="f2b10-222">Kopiera konfigurationsfilen på både virtuella datorer och starta Corosync i båda noderna:</span><span class="sxs-lookup"><span data-stu-id="f2b10-222">Copy this configuration file on both VMs and start Corosync in both nodes:</span></span>

    sudo service start corosync

<span data-ttu-id="f2b10-223">Hello kluster bör upprättas i hello aktuella ringen strax efter hello-tjänsten startas, och kvorum bör bildas.</span><span class="sxs-lookup"><span data-stu-id="f2b10-223">Shortly after starting hello service, hello cluster should be established in hello current ring, and quorum should be constituted.</span></span> <span data-ttu-id="f2b10-224">Vi kan kontrollera den här funktionen genom att granska loggarna eller genom att köra hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="f2b10-224">We can check this functionality by reviewing logs or by running hello following code:</span></span>

    sudo corosync-quorumtool –l

<span data-ttu-id="f2b10-225">Utdata liknande toohello följande bild visas:</span><span class="sxs-lookup"><span data-stu-id="f2b10-225">You will see output similar toohello following image:</span></span>

![exempel på utdata från corosync quorumtool - l](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a><span data-ttu-id="f2b10-227">Ställ in Pacemaker</span><span class="sxs-lookup"><span data-stu-id="f2b10-227">Set up Pacemaker</span></span>
<span data-ttu-id="f2b10-228">Pacemaker använder hello klustret toomonitor för resurser, definiera när primärfärgerna kraschar och växla toosecondaries dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="f2b10-228">Pacemaker uses hello cluster toomonitor for resources, define when primaries go down, and switch those resources toosecondaries.</span></span> <span data-ttu-id="f2b10-229">Du kan definiera resurser från en uppsättning tillgängliga skript eller LSB (init-liknande) skript, bland andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="f2b10-229">Resources can be defined from a set of available scripts or from LSB (init-like) scripts, among other choices.</span></span>

<span data-ttu-id="f2b10-230">Vi vill Pacemaker för ”egna” Hej DRBD resurs hello monteringspunkt och hello MySQL-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f2b10-230">We want Pacemaker too"own" hello DRBD resource, hello mount point, and hello MySQL service.</span></span> <span data-ttu-id="f2b10-231">Om Pacemaker kan aktivera och inaktivera DRBD, montera och demontera den, och sedan starta och stoppa MySQL i hello i rätt ordning när ett fel inträffar med hello primära installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="f2b10-231">If Pacemaker can turn on and off DRBD, mount and unmount it, and then start and stop MySQL in hello right order when something bad happens with hello primary, setup is complete.</span></span>

<span data-ttu-id="f2b10-232">När du installerar Pacemaker ska konfigurationen vara enkelt, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="f2b10-232">When you first install Pacemaker, your configuration should be simple enough, something like:</span></span>

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. <span data-ttu-id="f2b10-233">Kontrollera hello-konfigurationen genom att köra `sudo crm configure show`.</span><span class="sxs-lookup"><span data-stu-id="f2b10-233">Check hello configuration by running `sudo crm configure show`.</span></span>
2. <span data-ttu-id="f2b10-234">Skapa en fil (t.ex. `/tmp/cluster.conf`) med hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="f2b10-234">Then create a file (like `/tmp/cluster.conf`) with hello following resources:</span></span>

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. <span data-ttu-id="f2b10-235">Läsa in hello-filen i hello-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f2b10-235">Load hello file into hello configuration.</span></span> <span data-ttu-id="f2b10-236">Du behöver bara toodo detta i en nod.</span><span class="sxs-lookup"><span data-stu-id="f2b10-236">You only need toodo this in one node.</span></span>

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. <span data-ttu-id="f2b10-237">Kontrollera att Pacemaker startar vid start i båda noderna:</span><span class="sxs-lookup"><span data-stu-id="f2b10-237">Make sure that Pacemaker starts at boot in both nodes:</span></span>

        sudo update-rc.d pacemaker defaults

5. <span data-ttu-id="f2b10-238">Med hjälp av `sudo crm_mon –L`, kontrollera att en av noderna har blivit hello-master för hello kluster och kör alla hello-resurser.</span><span class="sxs-lookup"><span data-stu-id="f2b10-238">By using `sudo crm_mon –L`, verify that one of your nodes has become hello master for hello cluster and is running all hello resources.</span></span> <span data-ttu-id="f2b10-239">Du kan använda montera och ps toocheck hello resurser med.</span><span class="sxs-lookup"><span data-stu-id="f2b10-239">You can use mount and ps toocheck that hello resources are running.</span></span>

<span data-ttu-id="f2b10-240">följande skärmbild visar hello `crm_mon` med en nod som stoppats (avsluta genom att välja Ctrl + C):</span><span class="sxs-lookup"><span data-stu-id="f2b10-240">hello following screenshot shows `crm_mon` with one node stopped (exit by selecting Ctrl+C):</span></span>

![crm_mon noden stoppades](./media/mysql-cluster/image002.png)

<span data-ttu-id="f2b10-242">Den här skärmbilden visar noder, en överordnad och en slav:</span><span class="sxs-lookup"><span data-stu-id="f2b10-242">This screenshot shows both nodes, one master and one slave:</span></span>

![crm_mon operativa över-/ underordnade](./media/mysql-cluster/image003.png)

## <a name="testing"></a><span data-ttu-id="f2b10-244">Testning</span><span class="sxs-lookup"><span data-stu-id="f2b10-244">Testing</span></span>
<span data-ttu-id="f2b10-245">Du är redo för att simulera en automatisk redundans.</span><span class="sxs-lookup"><span data-stu-id="f2b10-245">You're ready for an automatic failover simulation.</span></span> <span data-ttu-id="f2b10-246">Det finns två sätt toodo detta: mjuk och hårddisk.</span><span class="sxs-lookup"><span data-stu-id="f2b10-246">There are two ways toodo this: soft and hard.</span></span>

<span data-ttu-id="f2b10-247">hello mjuka sätt funktionen hello klustrets avstängning: ``crm_standby -U `uname -n` -v on``.</span><span class="sxs-lookup"><span data-stu-id="f2b10-247">hello soft way uses hello cluster's shutdown function: ``crm_standby -U `uname -n` -v on``.</span></span> <span data-ttu-id="f2b10-248">Om du använder det här på hello master tar hello slavserver över.</span><span class="sxs-lookup"><span data-stu-id="f2b10-248">If you use this on hello master, hello slave takes over.</span></span> <span data-ttu-id="f2b10-249">Kom ihåg tooset denna tillbaka toooff.</span><span class="sxs-lookup"><span data-stu-id="f2b10-249">Remember tooset this back toooff.</span></span> <span data-ttu-id="f2b10-250">Om du inte visas crm_mon en nod i vänteläge.</span><span class="sxs-lookup"><span data-stu-id="f2b10-250">If you don't, crm_mon will show one node on standby.</span></span>

<span data-ttu-id="f2b10-251">hello hårda sätt stängs ned hello primära virtuella datorn (hadb01) via hello-portalen eller genom att ändra hello är på hello VM (det vill säga stopp, avstängning).</span><span class="sxs-lookup"><span data-stu-id="f2b10-251">hello hard way is shutting down hello primary VM (hadb01) via hello portal or by changing hello runlevel on hello VM (that is, halt, shutdown).</span></span> <span data-ttu-id="f2b10-252">Detta hjälper Corosync och Pacemaker av signalering som hello master gå nedåt.</span><span class="sxs-lookup"><span data-stu-id="f2b10-252">This helps Corosync and Pacemaker by signaling that hello master's going down.</span></span> <span data-ttu-id="f2b10-253">Du kan testa detta (användbart för underhållsfönster), men du kan också tvinga hello scenario genom att frysa hello VM.</span><span class="sxs-lookup"><span data-stu-id="f2b10-253">You can test this (useful for maintenance windows), but you can also force hello scenario by freezing hello VM.</span></span>

## <a name="stonith"></a><span data-ttu-id="f2b10-254">STONITH</span><span class="sxs-lookup"><span data-stu-id="f2b10-254">STONITH</span></span>
<span data-ttu-id="f2b10-255">Det bör vara möjligt tooissue en VM avstängning via hello Azure CLI i stället för ett STONITH skript som styr en fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="f2b10-255">It should be possible tooissue a VM shutdown via hello Azure CLI in lieu of a STONITH script that controls a physical device.</span></span> <span data-ttu-id="f2b10-256">Du kan använda `/usr/lib/stonith/plugins/external/ssh` som en bas- och aktivera STONITH i hello klusterkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="f2b10-256">You can use `/usr/lib/stonith/plugins/external/ssh` as a base and enable STONITH in hello cluster's configuration.</span></span> <span data-ttu-id="f2b10-257">Azure CLI globalt ska installeras och hello Publiceringsinställningar och profil ska läsas för hello klustrets användare.</span><span class="sxs-lookup"><span data-stu-id="f2b10-257">Azure CLI should be globally installed, and hello publish settings and profile should be loaded for hello cluster's user.</span></span>

<span data-ttu-id="f2b10-258">Exempelkod för hello resursen är tillgänglig på [GitHub](https://github.com/bureado/aztonith).</span><span class="sxs-lookup"><span data-stu-id="f2b10-258">Sample code for hello resource is available on [GitHub](https://github.com/bureado/aztonith).</span></span> <span data-ttu-id="f2b10-259">Ändra hello klusterkonfigurationen genom att lägga till hello följande för`sudo crm configure`:</span><span class="sxs-lookup"><span data-stu-id="f2b10-259">Change hello cluster's configuration by adding hello following too`sudo crm configure`:</span></span>

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> <span data-ttu-id="f2b10-260">hello skript utföra inte upp/ned kontroller.</span><span class="sxs-lookup"><span data-stu-id="f2b10-260">hello script doesn't perform up/down checks.</span></span> <span data-ttu-id="f2b10-261">hello ursprungliga SSH resursen hade 15 ping-kontroller, men tiden för återställning för en virtuell dator i Azure kan vara mer variabel.</span><span class="sxs-lookup"><span data-stu-id="f2b10-261">hello original SSH resource had 15 ping checks, but recovery time for an Azure VM might be more variable.</span></span>

## <a name="limitations"></a><span data-ttu-id="f2b10-262">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="f2b10-262">Limitations</span></span>
<span data-ttu-id="f2b10-263">hello följande begränsningar gäller:</span><span class="sxs-lookup"><span data-stu-id="f2b10-263">hello following limitations apply:</span></span>

* <span data-ttu-id="f2b10-264">Hej linbit DRBD resurs skriptet som hanterar DRBD som en resurs i Pacemaker använder `drbdadm down` när du stänger en nod, även om bara hello nod försätts i vänteläge.</span><span class="sxs-lookup"><span data-stu-id="f2b10-264">hello linbit DRBD resource script that manages DRBD as a resource in Pacemaker uses `drbdadm down` when shutting down a node, even if hello node is just going on standby.</span></span> <span data-ttu-id="f2b10-265">Detta är inte idealiskt eftersom hello slavserver inte kommer att synkronisera hello DRBD resurs medan hello master hämtar skrivningar.</span><span class="sxs-lookup"><span data-stu-id="f2b10-265">This is not ideal because hello slave will not be synchronizing hello DRBD resource while hello master gets writes.</span></span> <span data-ttu-id="f2b10-266">Om hello master inte misslyckas graciously, kan hello slavserver ta över en äldre tillståndet för filsystemet.</span><span class="sxs-lookup"><span data-stu-id="f2b10-266">If hello master does not fail graciously, hello slave can take over an older file system state.</span></span> <span data-ttu-id="f2b10-267">Det finns två möjliga sätt att lösa det här:</span><span class="sxs-lookup"><span data-stu-id="f2b10-267">There are two potential ways of solving this:</span></span>
  * <span data-ttu-id="f2b10-268">Framtvinga en `drbdadm up r0` i alla noder i klustret via ett lokalt (inte clusterized) watchdog</span><span class="sxs-lookup"><span data-stu-id="f2b10-268">Enforcing a `drbdadm up r0` in all cluster nodes via a local (not clusterized) watchdog</span></span>
  * <span data-ttu-id="f2b10-269">Redigera hello linbit DRBD skript, se till att `down` inte anropas`/usr/lib/ocf/resource.d/linbit/drbd`</span><span class="sxs-lookup"><span data-stu-id="f2b10-269">Editing hello linbit DRBD script, making sure that `down` is not called in `/usr/lib/ocf/resource.d/linbit/drbd`</span></span>
* <span data-ttu-id="f2b10-270">hello belastningsutjämnaren måste vara minst 5 sekunder toorespond så att program ska vara kluster och vara mer tolerant för timeout.</span><span class="sxs-lookup"><span data-stu-id="f2b10-270">hello load balancer needs at least five seconds toorespond, so applications should be cluster-aware and be more tolerant of timeout.</span></span> <span data-ttu-id="f2b10-271">Andra arkitekturer som app-köer och fråga middlewares kan också.</span><span class="sxs-lookup"><span data-stu-id="f2b10-271">Other architectures, like in-app queues and query middlewares, can also help.</span></span>
* <span data-ttu-id="f2b10-272">MySQL justera är nödvändiga tooensure som skrivning görs i en hanterbar takt och cacheminnen är en toodisk så ofta som möjligt toominimize minne dataförlust.</span><span class="sxs-lookup"><span data-stu-id="f2b10-272">MySQL tuning is necessary tooensure that writing is done at a manageable pace and caches are flushed toodisk as frequently as possible toominimize memory loss.</span></span>
* <span data-ttu-id="f2b10-273">Skriva prestanda beror på virtuell dator kopplas samman i hello virtuell växel eftersom det är hello mekanismen som används av DRBD tooreplicate hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f2b10-273">Write performance is dependent in VM interconnect in hello virtual switch because this is hello mechanism used by DRBD tooreplicate hello device.</span></span>
