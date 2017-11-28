---
title: aaaRun MariaDB (MySQL) kluster i Azure | Microsoft Docs
description: "Skapa en MariaDB + Galera MySQL-kluster på Azure virtual machines"
services: virtual-machines-linux
documentationcenter: 
author: sabbour
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: f9a4d6c45d76478a8a3526b407c7bbe6aeb40423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a><span data-ttu-id="c3d6a-103">MariaDB (MySQL) kluster: Azure självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="c3d6a-103">MariaDB (MySQL) cluster: Azure tutorial</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c3d6a-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager](../../../resource-manager-deployment-model.md) och klassisk.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="c3d6a-105">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-105">This article covers hello classic deployment model.</span></span> <span data-ttu-id="c3d6a-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Azure Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-106">Microsoft recommends that most new deployments use hello Azure Resource Manager model.</span></span>

> [!NOTE]
> <span data-ttu-id="c3d6a-107">MariaDB Enterprise kluster är nu tillgänglig i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-107">MariaDB Enterprise cluster is now available in hello Azure Marketplace.</span></span> <span data-ttu-id="c3d6a-108">hello nytt erbjudande distribuera automatiskt ett MariaDB Galera kluster på Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-108">hello new offering will automatically deploy a MariaDB Galera cluster on Azure Resource Manager.</span></span> <span data-ttu-id="c3d6a-109">Du bör använda hello nytt erbjudande från [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span><span class="sxs-lookup"><span data-stu-id="c3d6a-109">You should use hello new offering from [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span></span>
>
>

<span data-ttu-id="c3d6a-110">Den här artikeln beskrivs hur du toocreate flera Master [Galera](http://galeracluster.com/products/) kluster med [MariaDBs](https://mariadb.org/en/about/) (en robust, skalbara och tillförlitliga gör att installera ersättning för MySQL) toowork i en miljö med hög tillgänglighet på Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-110">This article shows you how toocreate a multi-Master [Galera](http://galeracluster.com/products/) cluster of [MariaDBs](https://mariadb.org/en/about/) (a robust, scalable, and reliable drop-in replacement for MySQL) toowork in a highly available environment on Azure virtual machines.</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="c3d6a-111">Översikt över arkitekturen</span><span class="sxs-lookup"><span data-stu-id="c3d6a-111">Architecture overview</span></span>
<span data-ttu-id="c3d6a-112">Den här artikeln beskriver hur toocomplete hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-112">This article describes how toocomplete hello following steps:</span></span>

- <span data-ttu-id="c3d6a-113">Skapa ett kluster med tre noder.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-113">Create a three-node cluster.</span></span>
- <span data-ttu-id="c3d6a-114">Separata hello datadiskar från hello OS-disken.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-114">Separate hello data disks from hello OS disk.</span></span>
- <span data-ttu-id="c3d6a-115">Skapa hello datadiskar i RAID-0/stripe inställningen tooincrease IOPS.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-115">Create hello data disks in RAID-0/striped setting tooincrease IOPS.</span></span>
- <span data-ttu-id="c3d6a-116">Använd Azure belastningsutjämnare toobalance hello belastningen för hello tre noder.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-116">Use Azure Load Balancer toobalance hello load for hello three nodes.</span></span>
- <span data-ttu-id="c3d6a-117">toominimize återkommande fungerar, skapa en VM-avbildning som innehåller MariaDB + Galera och använda den toocreate hello andra kluster virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-117">toominimize repetitive work, create a VM image that contains MariaDB + Galera and use it toocreate hello other cluster VMs.</span></span>

![Systemarkitektur](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> <span data-ttu-id="c3d6a-119">Det här avsnittet använder hello [Azure CLI](../../../cli-install-nodejs.md) verktyg, så se till att toodownload dem och ansluter dem tooyour Azure-prenumeration bl.a toohello instruktioner.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-119">This topic uses hello [Azure CLI](../../../cli-install-nodejs.md) tools, so make sure toodownload them and connect them tooyour Azure subscription according toohello instructions.</span></span> <span data-ttu-id="c3d6a-120">Om du behöver en referens toohello kommandon i hello Azure CLI finns hello [Azure CLI kommandoreferens](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="c3d6a-120">If you need a reference toohello commands available in hello Azure CLI, see hello [Azure CLI command reference](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="c3d6a-121">Du måste också för[skapa en SSH-nyckel för autentisering] och anteckna hello .pem filens plats.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-121">You will also need too[create an SSH key for authentication] and make note of hello .pem file location.</span></span>
>
>

## <a name="create-hello-template"></a><span data-ttu-id="c3d6a-122">Skapa hello mall</span><span class="sxs-lookup"><span data-stu-id="c3d6a-122">Create hello template</span></span>
### <a name="infrastructure"></a><span data-ttu-id="c3d6a-123">Infrastruktur</span><span class="sxs-lookup"><span data-stu-id="c3d6a-123">Infrastructure</span></span>
1. <span data-ttu-id="c3d6a-124">Skapa en tillhörighetsgrupp toohold hello resurser tillsammans.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-124">Create an affinity group toohold hello resources together.</span></span>

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. <span data-ttu-id="c3d6a-125">Skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-125">Create a virtual network.</span></span>

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. <span data-ttu-id="c3d6a-126">Skapa en storage-konto toohost våra diskar.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-126">Create a storage account toohost all our disks.</span></span> <span data-ttu-id="c3d6a-127">Du bör placera mer än 40 hårt belastat diskar på hello samma storage-konto tooavoid träffa hello 20 000 IOPS lagringsgräns konto.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-127">You shouldn't place more than 40 heavily used disks on hello same storage account tooavoid hitting hello 20,000 IOPS storage account limit.</span></span> <span data-ttu-id="c3d6a-128">I det här fallet är väl under denna gräns så att du lagrar allt på hello samma konto för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-128">In this case, you're well below that limit, so you'll store everything on hello same account for simplicity.</span></span>

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. <span data-ttu-id="c3d6a-129">Hitta hello namnet på hello CentOS 7 avbildning av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-129">Find hello name of hello CentOS 7 virtual machine image.</span></span>

        azure vm image list | findstr CentOS
   <span data-ttu-id="c3d6a-130">hello utdata blir något som liknar `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-130">hello output will be something like `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span></span>

   <span data-ttu-id="c3d6a-131">Använd det här namnet i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-131">Use that name in hello following step.</span></span>
5. <span data-ttu-id="c3d6a-132">Skapa mall för virtuell dator hello och Ersätt /path/to/key.pem med hello sökvägen där du sparade hello genereras .pem SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-132">Create hello VM template and replace /path/to/key.pem with hello path where you stored hello generated .pem SSH key.</span></span>

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. <span data-ttu-id="c3d6a-133">Koppla fyra 500 GB data diskar toohello VM för användning i hello RAID-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-133">Attach four 500-GB data disks toohello VM for use in hello RAID configuration.</span></span>

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. <span data-ttu-id="c3d6a-134">Använda SSH toosign i toohello mall för virtuell dator som du skapade i mariadbhatemplate.cloudapp.net:22 och ansluta med hjälp av din privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-134">Use SSH toosign in toohello template VM that you created at mariadbhatemplate.cloudapp.net:22, and connect by using your private key.</span></span>

### <a name="software"></a><span data-ttu-id="c3d6a-135">Programvara</span><span class="sxs-lookup"><span data-stu-id="c3d6a-135">Software</span></span>
1. <span data-ttu-id="c3d6a-136">Hämta hello rot.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-136">Get hello root.</span></span>

        sudo su

2. <span data-ttu-id="c3d6a-137">Installera stöd för RAID:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-137">Install RAID support:</span></span>

    <span data-ttu-id="c3d6a-138">a.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-138">a.</span></span> <span data-ttu-id="c3d6a-139">Installera mdadm.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-139">Install mdadm.</span></span>

              yum install mdadm

    <span data-ttu-id="c3d6a-140">b.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-140">b.</span></span> <span data-ttu-id="c3d6a-141">Skapa hello RAID0/stripe-konfigurationen med ett EXT4 filsystem.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-141">Create hello RAID0/stripe configuration with an EXT4 file system.</span></span>

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    <span data-ttu-id="c3d6a-142">c.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-142">c.</span></span> <span data-ttu-id="c3d6a-143">Skapa hello avbildningskatalog punkt.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-143">Create hello mount point directory.</span></span>

              mkdir /mnt/data
    <span data-ttu-id="c3d6a-144">d.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-144">d.</span></span> <span data-ttu-id="c3d6a-145">Hämta hello UUID för hello nyskapad RAID-enhet.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-145">Retrieve hello UUID of hello newly created RAID device.</span></span>

              blkid | grep /dev/md0
    <span data-ttu-id="c3d6a-146">e.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-146">e.</span></span> <span data-ttu-id="c3d6a-147">Redigera /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-147">Edit /etc/fstab.</span></span>

              vi /etc/fstab
    <span data-ttu-id="c3d6a-148">f.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-148">f.</span></span> <span data-ttu-id="c3d6a-149">Lägg till hello enheten tooenable automatisk montering på omstart, Ersätt hello UUID med hello-värde hämtades från hello tidigare **blkid** kommando.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-149">Add hello device tooenable auto mounting on reboot, replacing hello UUID with hello value obtained from hello previous **blkid** command.</span></span>

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    <span data-ttu-id="c3d6a-150">g.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-150">g.</span></span> <span data-ttu-id="c3d6a-151">Montera hello ny partition.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-151">Mount hello new partition.</span></span>

              mount /mnt/data

3. <span data-ttu-id="c3d6a-152">Installera MariaDB.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-152">Install MariaDB.</span></span>

    <span data-ttu-id="c3d6a-153">a.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-153">a.</span></span> <span data-ttu-id="c3d6a-154">Skapa hello MariaDB.repo fil.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-154">Create hello MariaDB.repo file.</span></span>

                vi /etc/yum.repos.d/MariaDB.repo

    <span data-ttu-id="c3d6a-155">b.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-155">b.</span></span> <span data-ttu-id="c3d6a-156">Fyll hello lagringsplatsen filen med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-156">Fill hello repo file with hello following content:</span></span>

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    <span data-ttu-id="c3d6a-157">c.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-157">c.</span></span> <span data-ttu-id="c3d6a-158">tooavoid konflikter, ta bort befintliga username@Domain och mariadb-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-158">tooavoid conflicts, remove existing postfix and mariadb-libs.</span></span>

           yum remove postfix mariadb-libs-*
    <span data-ttu-id="c3d6a-159">d.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-159">d.</span></span> <span data-ttu-id="c3d6a-160">Installera MariaDB med Galera.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-160">Install MariaDB with Galera.</span></span>

           yum install MariaDB-Galera-server MariaDB-client galera

4. <span data-ttu-id="c3d6a-161">Flytta hello MySQL data directory toohello RAID block-enhet.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-161">Move hello MySQL data directory toohello RAID block device.</span></span>

    <span data-ttu-id="c3d6a-162">a.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-162">a.</span></span> <span data-ttu-id="c3d6a-163">Kopiera hello MySQL-katalogen till den nya platsen och ta bort gamla hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-163">Copy hello current MySQL directory into its new location and remove hello old directory.</span></span>

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    <span data-ttu-id="c3d6a-164">b.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-164">b.</span></span> <span data-ttu-id="c3d6a-165">Ange behörigheter för hello ny katalog i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-165">Set permissions for hello new directory accordingly.</span></span>

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    <span data-ttu-id="c3d6a-166">c.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-166">c.</span></span> <span data-ttu-id="c3d6a-167">Skapa en symlink som pekar hello gamla toohello nya katalogplatsen på hello RAID-partitionen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-167">Create a symlink that points hello old directory toohello new location on hello RAID partition.</span></span>

           ln -s /mnt/data/mysql /var/lib/mysql

5. <span data-ttu-id="c3d6a-168">Eftersom [SELinux kommer att störa klusteråtgärder hello](http://galeracluster.com/documentation-webpages/configuration.html#selinux), är det nödvändigt toodisable för hello aktuell session.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-168">Because [SELinux interferes with hello cluster operations](http://galeracluster.com/documentation-webpages/configuration.html#selinux), it is necessary toodisable it for hello current session.</span></span> <span data-ttu-id="c3d6a-169">Redigera `/etc/selinux/config` toodisable den för efterföljande omstart.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-169">Edit `/etc/selinux/config` toodisable it for subsequent restarts.</span></span>

            setenforce 0

            then editing `/etc/selinux/config` tooset `SELINUX=permissive`
6. <span data-ttu-id="c3d6a-170">Validera MySQL körs.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-170">Validate MySQL runs.</span></span>

   <span data-ttu-id="c3d6a-171">a.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-171">a.</span></span> <span data-ttu-id="c3d6a-172">Starta MySQL.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-172">Start MySQL.</span></span>

           service mysql start
   <span data-ttu-id="c3d6a-173">b.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-173">b.</span></span> <span data-ttu-id="c3d6a-174">Skydda hello MySQL installationen, ange hello rotlösenordet, ta bort anonyma användare toodisable remote rot inloggning och ta bort hello Testa databas.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-174">Secure hello MySQL installation, set hello root password, remove anonymous users toodisable remote root login, and remove hello test database.</span></span>

           mysql_secure_installation
   <span data-ttu-id="c3d6a-175">c.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-175">c.</span></span> <span data-ttu-id="c3d6a-176">Skapa en användare på hello databas för klusteråtgärder och eventuellt för dina program.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-176">Create a user on hello database for cluster operations, and optionally for your applications.</span></span>

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* too'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   <span data-ttu-id="c3d6a-177">d.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-177">d.</span></span> <span data-ttu-id="c3d6a-178">Stoppa MySQL.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-178">Stop MySQL.</span></span>

            service mysql stop
7. <span data-ttu-id="c3d6a-179">Skapa en platshållare för konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-179">Create a configuration placeholder.</span></span>

   <span data-ttu-id="c3d6a-180">a.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-180">a.</span></span> <span data-ttu-id="c3d6a-181">Redigera hello MySQL configuration toocreate platshållare för hello klusterinställningar.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-181">Edit hello MySQL configuration toocreate a placeholder for hello cluster settings.</span></span> <span data-ttu-id="c3d6a-182">Ersätt inte hello ** `<Variables>` ** eller ta bort kommentarerna nu.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-182">Do not replace hello **`<Variables>`** or uncomment now.</span></span> <span data-ttu-id="c3d6a-183">Som sker när du har skapat en virtuell dator från den här mallen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-183">That will happen after you create a VM from this template.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="c3d6a-184">b.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-184">b.</span></span> <span data-ttu-id="c3d6a-185">Redigera hello ** [galera] ** avsnittet och ta bort.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-185">Edit hello **[galera]** section and clear it out.</span></span>

   <span data-ttu-id="c3d6a-186">c.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-186">c.</span></span> <span data-ttu-id="c3d6a-187">Redigera hello **[mariadb]** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-187">Edit hello **[mariadb]** section.</span></span>

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set too0.0.0.0, hello server listens tooremote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for hello SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set hello node name of this server
8. <span data-ttu-id="c3d6a-188">Öppna portar som krävs på hello-brandväggen med hjälp av FirewallD på CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-188">Open required ports on hello firewall by using FirewallD on CentOS 7.</span></span>

   * <span data-ttu-id="c3d6a-189">MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="c3d6a-189">MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span></span>
   * <span data-ttu-id="c3d6a-190">GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="c3d6a-190">GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span></span>
   * <span data-ttu-id="c3d6a-191">GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="c3d6a-191">GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span></span>
   * <span data-ttu-id="c3d6a-192">RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="c3d6a-192">RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span></span>
   * <span data-ttu-id="c3d6a-193">Läs in hello brandväggen:`firewall-cmd --reload`</span><span class="sxs-lookup"><span data-stu-id="c3d6a-193">Reload hello firewall: `firewall-cmd --reload`</span></span>

9. <span data-ttu-id="c3d6a-194">Optimera hello system för prestanda.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-194">Optimize hello system for performance.</span></span> <span data-ttu-id="c3d6a-195">Mer information finns i [strategi för prestandajustering](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="c3d6a-195">For more information, see [performance tuning strategy](optimize-mysql.md).</span></span>

   <span data-ttu-id="c3d6a-196">a.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-196">a.</span></span> <span data-ttu-id="c3d6a-197">Redigera konfigurationsfilen för hello MySQL igen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-197">Edit hello MySQL configuration file again.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="c3d6a-198">b.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-198">b.</span></span> <span data-ttu-id="c3d6a-199">Redigera hello **[mariadb]** avsnittet och Lägg till hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-199">Edit hello **[mariadb]** section and append hello following content:</span></span>

   > [!NOTE]
   > <span data-ttu-id="c3d6a-200">Vi rekommenderar att innodb\_buffert\_pool_size är 70 procent av den Virtuella datorns minne.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-200">We recommend that innodb\_buffer\_pool_size is 70 percent of your VM's memory.</span></span> <span data-ttu-id="c3d6a-201">I det här exemplet, har den angetts på 2,45 GB för medelstora hello Azure VM med 3.5 GB RAM.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-201">In this example, it has been set at 2.45 GB for hello medium Azure VM with 3.5 GB of RAM.</span></span>
   >
   >

           innodb_buffer_pool_size = 2508M # hello buffer pool contains buffered data and hello index. This is usually set too70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give hello server more time toorecycle idled connections
           innodb_file_per_table = 1 # Speed up hello table space transmission and optimize hello debris management performance
           innodb_log_buffer_size = 128M # hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit
           innodb_flush_log_at_trx_commit = 2 # hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. <span data-ttu-id="c3d6a-202">Stoppa MySQL, inaktivera MySQL-tjänsten körs på Start tooavoid störa hello klustret när du lägger till en nod och ta bort etableringen hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-202">Stop MySQL, disable MySQL service from running on startup tooavoid disrupting hello cluster when adding a node, and deprovision hello machine.</span></span>

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. <span data-ttu-id="c3d6a-203">Avbilda hello VM hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-203">Capture hello VM through hello portal.</span></span> <span data-ttu-id="c3d6a-204">(För närvarande [utfärda #1268 i hello Azure CLI-verktygen](https://github.com/Azure/azure-xplat-cli/issues/1268) beskriver hello faktum att bilder som avbildas av hello Azure CLI-verktygen inte kan fångas hello kopplade datadiskar.)</span><span class="sxs-lookup"><span data-stu-id="c3d6a-204">(Currently, [issue #1268 in hello Azure CLI tools](https://github.com/Azure/azure-xplat-cli/issues/1268) describes hello fact that images captured by hello Azure CLI tools do not capture hello attached data disks.)</span></span>

    <span data-ttu-id="c3d6a-205">a.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-205">a.</span></span> <span data-ttu-id="c3d6a-206">Stäng av datorn hello hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-206">Shut down hello machine through hello portal.</span></span>

    <span data-ttu-id="c3d6a-207">b.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-207">b.</span></span> <span data-ttu-id="c3d6a-208">Klicka på **avbilda** och ange hello Avbildningsnamnet som **mariadb galera avbildning**.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-208">Click **Capture** and specify hello image name as **mariadb-galera-image**.</span></span> <span data-ttu-id="c3d6a-209">Ange en beskrivning och markera ”jag har kört waagent”.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-209">Provide a description and check "I have run waagent."</span></span>
      
      ![Avbilda hello virtuella datorn](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-hello-cluster"></a><span data-ttu-id="c3d6a-211">Skapa hello-kluster</span><span class="sxs-lookup"><span data-stu-id="c3d6a-211">Create hello cluster</span></span>
<span data-ttu-id="c3d6a-212">Skapa tre virtuella datorer med hello mallen du skapat, och sedan konfigurera och starta hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-212">Create three VMs with hello template you created, and then configure and start hello cluster.</span></span>

1. <span data-ttu-id="c3d6a-213">Skapa hello första CentOS 7-VM från hello mariadb-galera-avbildning bild du skapade, tillhandahåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-213">Create hello first CentOS 7 VM from hello mariadb-galera-image image you created, providing hello following information:</span></span>

 - <span data-ttu-id="c3d6a-214">Virtuella nätverksnamnet: mariadbvnet</span><span class="sxs-lookup"><span data-stu-id="c3d6a-214">Virtual network name: mariadbvnet</span></span>
 - <span data-ttu-id="c3d6a-215">Undernät: mariadb</span><span class="sxs-lookup"><span data-stu-id="c3d6a-215">Subnet: mariadb</span></span>
 - <span data-ttu-id="c3d6a-216">Datorn storlek: medelstor</span><span class="sxs-lookup"><span data-stu-id="c3d6a-216">Machine size: medium</span></span>
 - <span data-ttu-id="c3d6a-217">Molntjänstnamnet: mariadbha (eller namnet du vill toobe som nås via mariadbha.cloudapp.net)</span><span class="sxs-lookup"><span data-stu-id="c3d6a-217">Cloud service name: mariadbha (or whatever name you want toobe accessed through mariadbha.cloudapp.net)</span></span>
 - <span data-ttu-id="c3d6a-218">Datornamn: mariadb1</span><span class="sxs-lookup"><span data-stu-id="c3d6a-218">Machine name: mariadb1</span></span>
 - <span data-ttu-id="c3d6a-219">Användarnamn: azureuser</span><span class="sxs-lookup"><span data-stu-id="c3d6a-219">Username: azureuser</span></span>
 - <span data-ttu-id="c3d6a-220">SSH-åtkomst: aktiverat</span><span class="sxs-lookup"><span data-stu-id="c3d6a-220">SSH access: enabled</span></span>
 - <span data-ttu-id="c3d6a-221">Skicka hello SSH certifikat PEM-filen och ersätter /path/to/key.pem med hello sökvägen där du sparade hello genereras .pem SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-221">Passing hello SSH certificate .pem file and replacing /path/to/key.pem with hello path where you stored hello generated .pem SSH key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c3d6a-222">hello följande kommandon delas över flera rader för tydlighetens skull, men du bör ange varje som en rad.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-222">hello following commands are split over multiple lines for clarity, but you should enter each as one line.</span></span>
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
2. <span data-ttu-id="c3d6a-223">Skapa två flera virtuella datorer genom att ansluta dem toohello mariadbha Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-223">Create two more virtual machines by connecting them toohello mariadbha cloud service.</span></span> <span data-ttu-id="c3d6a-224">Ändra hello VM namn och hello SSH-port tooa unika-port som inte står i konflikt med andra virtuella datorer i hello samma tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-224">Change hello VM name and hello SSH port tooa unique port not conflicting with other VMs in hello same cloud service.</span></span>

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  <span data-ttu-id="c3d6a-225">För MariaDB3:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-225">For MariaDB3:</span></span>

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
3. <span data-ttu-id="c3d6a-226">Du behöver tooget hello interna IP-adressen för varje hello tre virtuella datorer för hello nästa steg:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-226">You will need tooget hello internal IP address of each of hello three VMs for hello next step:</span></span>

    ![Hämtar IP-adress](./media/mariadb-mysql-cluster/IP.png)
4. <span data-ttu-id="c3d6a-228">Använd SSH toosign i toohello tre virtuella datorer och redigera hello konfigurationsfilen på var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-228">Use SSH toosign in toohello three VMs and edit hello configuration file on each of them.</span></span>

        sudo vi /etc/my.cnf.d/server.cnf

    <span data-ttu-id="c3d6a-229">Ta bort kommentarerna ** `wsrep_cluster_name` ** och ** `wsrep_cluster_address` ** genom att ta bort hello ** # ** hello början av hello rad.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-229">Uncomment **`wsrep_cluster_name`** and **`wsrep_cluster_address`** by removing hello **#** at hello beginning of hello line.</span></span>
    <span data-ttu-id="c3d6a-230">Dessutom kan ersätta ** `<ServerIP>` ** i ** `wsrep_node_address` ** och ** `<NodeName>` ** i ** `wsrep_node_name` ** med hello Virtuella datorns IP-adress, namn, och Avkommentera dessa rader samt.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-230">Additionally, replace **`<ServerIP>`** in **`wsrep_node_address`** and **`<NodeName>`** in **`wsrep_node_name`** with hello VM's IP address and name, respectively, and uncomment those lines as well.</span></span>
5. <span data-ttu-id="c3d6a-231">Starta hello kluster på MariaDB1 och låt den körs vid start.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-231">Start hello cluster on MariaDB1 and let it run at startup.</span></span>

        sudo service mysql bootstrap
        chkconfig mysql on
6. <span data-ttu-id="c3d6a-232">Starta MySQL på MariaDB2 och MariaDB3 och låt den körs vid start.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-232">Start MySQL on MariaDB2 and MariaDB3 and let it run at startup.</span></span>

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-hello-cluster"></a><span data-ttu-id="c3d6a-233">Läs in saldo hello kluster</span><span class="sxs-lookup"><span data-stu-id="c3d6a-233">Load balance hello cluster</span></span>
<span data-ttu-id="c3d6a-234">När du har skapat hello klustrade virtuella datorer kan du lagt till dem i en tillgänglighetsuppsättning kallas clusteravset tooensure de placerades i olika fel- och update-domäner och att Azure aldrig har Underhåll på alla datorer på samma gång.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-234">When you created hello clustered VMs, you added them into an availability set called clusteravset tooensure that they were put on different fault and update domains and that Azure never does maintenance on all machines at once.</span></span> <span data-ttu-id="c3d6a-235">Den här konfigurationen uppfyller hello toobe stöds av hello Azure servicenivåavtal (SLA).</span><span class="sxs-lookup"><span data-stu-id="c3d6a-235">This configuration meets hello requirements toobe supported by hello Azure service level agreement (SLA).</span></span>

<span data-ttu-id="c3d6a-236">Nu ska du använda Azure belastningsutjämnare toobalance begäranden mellan hello tre noder.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-236">Now use Azure Load Balancer toobalance requests between hello three nodes.</span></span>

<span data-ttu-id="c3d6a-237">Kör följande kommandon på datorn med hjälp av hello Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-237">Run hello following commands on your machine by using hello Azure CLI.</span></span>

<span data-ttu-id="c3d6a-238">Hej kommandostruktur parametrar är:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span><span class="sxs-lookup"><span data-stu-id="c3d6a-238">hello command parameters structure is: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span></span>

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

<span data-ttu-id="c3d6a-239">hello CLI anger hello belastningen belastningsutjämnaren avsökningen intervall too15 i sekunder, som kan vara lite för långt.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-239">hello CLI sets hello load balancer probe interval too15 seconds, which might be a bit too long.</span></span> <span data-ttu-id="c3d6a-240">Ändra den i portalen hello under **slutpunkter** för någon av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-240">Change it in hello portal under **Endpoints** for any of hello VMs.</span></span>

![Redigera slutpunkt](./media/mariadb-mysql-cluster/Endpoint.PNG)

<span data-ttu-id="c3d6a-242">Välj **Reconfigure hello Load-Balanced ange**.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-242">Select **Reconfigure hello Load-Balanced Set**.</span></span>

![Konfigurera om hello belastningsutjämnad uppsättning](./media/mariadb-mysql-cluster/Endpoint2.PNG)

<span data-ttu-id="c3d6a-244">Ändra **avsökning intervall** too5 sekunder och spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-244">Change **Probe Interval** too5 seconds and save your changes.</span></span>

![Ändra avsökningsintervall](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-hello-cluster"></a><span data-ttu-id="c3d6a-246">Verifiera hello-kluster</span><span class="sxs-lookup"><span data-stu-id="c3d6a-246">Validate hello cluster</span></span>
<span data-ttu-id="c3d6a-247">hello tunga arbetet görs.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-247">hello hard work is done.</span></span> <span data-ttu-id="c3d6a-248">hello klustret ska finnas nu tillgänglig för `mariadbha.cloudapp.net:3306`som träffar hello belastningsutjämnare och väg begäranden mellan hello tre virtuella datorer smidigt och effektivt sätt.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-248">hello cluster should be now accessible at `mariadbha.cloudapp.net:3306`, which hits hello load balancer and route requests between hello three VMs smoothly and efficiently.</span></span>

<span data-ttu-id="c3d6a-249">Använd din favorit MySQL klienten tooconnect eller ansluter från en hello VMs tooverify som fungerar som det här klustret.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-249">Use your favorite MySQL client tooconnect, or connect from one of hello VMs tooverify that this cluster is working.</span></span>

     mysql -u cluster -h mariadbha.cloudapp.net -p

<span data-ttu-id="c3d6a-250">Sedan skapar en databas och fylla det med vissa data.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-250">Then create a database and populate it with some data.</span></span>

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

<span data-ttu-id="c3d6a-251">hello-databasen som du skapade returnerar hello följande tabell:</span><span class="sxs-lookup"><span data-stu-id="c3d6a-251">hello database you created returns hello following table:</span></span>

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="c3d6a-252">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3d6a-252">Next steps</span></span>
<span data-ttu-id="c3d6a-253">Du har skapat en tre noder MariaDB + Galera Högtillgängliga kluster på Azure virtuella datorer körs CentOS 7 i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-253">In this article, you created a three-node MariaDB + Galera highly available cluster on Azure virtual machines running CentOS 7.</span></span> <span data-ttu-id="c3d6a-254">hello VMs belastningsutjämnas med Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="c3d6a-254">hello VMs are load balanced with Azure Load Balancer.</span></span>

<span data-ttu-id="c3d6a-255">Du kanske vill toolook på [ett annat sätt toocluster MySQL på Linux](mysql-cluster.md) och sätt för[optimera och testa MySQL prestanda på virtuella Azure Linux-datorer](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="c3d6a-255">You might want toolook at [another way toocluster MySQL on Linux](mysql-cluster.md) and ways too[optimize and test MySQL performance on Azure Linux VMs](optimize-mysql.md).</span></span>

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating hello template]:#creating-the-template
[Creating hello cluster]:#creating-the-cluster
[Load balancing hello cluster]:#load-balancing-the-cluster
[Validating hello cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[Skapa en SSH-nyckel för autentisering]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in hello Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
