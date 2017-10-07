---
title: "aaaSet in MySQL på en Linux-VM i Azure | Microsoft Docs"
description: "Lär dig hur tooinstall hello MySQL stacken på en virtuell Linux-dator (Ubuntu eller RedHat familjen OS) i Azure"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: e47d0de7f0eb5bb873ad20e4bc35f1b5f8d33bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-mysql-on-azure"></a><span data-ttu-id="f4600-103">Hur tooinstall MySQL på Azure</span><span class="sxs-lookup"><span data-stu-id="f4600-103">How tooinstall MySQL on Azure</span></span>
<span data-ttu-id="f4600-104">I den här artikeln får du lära dig hur tooinstall och konfigurera MySQL på en Azure-dator som kör Linux.</span><span class="sxs-lookup"><span data-stu-id="f4600-104">In this article, you will learn how tooinstall and configure MySQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a><span data-ttu-id="f4600-105">Installera MySQL på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f4600-105">Install MySQL on your virtual machine</span></span>
> [!NOTE]
> <span data-ttu-id="f4600-106">Du måste redan ha en Microsoft Azure-dator som kör Linux i ordning toocomplete den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f4600-106">You must already have a Microsoft Azure virtual machine running Linux in order toocomplete this tutorial.</span></span> <span data-ttu-id="f4600-107">Finns det [virtuella Azure Linux-datorn kursen](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate och konfigurera en Linux-VM med `mysqlnode` som hello VM namn och `azureuser` som användare innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="f4600-107">Please see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate and set up a Linux VM with `mysqlnode` as hello VM name and `azureuser` as user before proceeding.</span></span>
> 
> 

<span data-ttu-id="f4600-108">I det här fallet Använd 3306 port som hello MySQL-port.</span><span class="sxs-lookup"><span data-stu-id="f4600-108">In this case, use 3306 port as hello MySQL port.</span></span>  

<span data-ttu-id="f4600-109">Ansluta toohello Linux VM du skapat via putty.</span><span class="sxs-lookup"><span data-stu-id="f4600-109">Connect toohello Linux VM you created via putty.</span></span> <span data-ttu-id="f4600-110">Hello första gången du använder virtuella Azure Linux-datorn, se hur toouse putty ansluter tooa Linux VM [här](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f4600-110">If this is hello first time you use Azure Linux VM, see how toouse putty connect tooa Linux VM [here](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f4600-111">Vi använder databasen paketet tooinstall MySQL5.6 som exempel i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f4600-111">We will use repository package tooinstall MySQL5.6 as an example in this article.</span></span> <span data-ttu-id="f4600-112">Faktiskt har MySQL5.6 flera förbättringar i prestanda än MySQL5.5.</span><span class="sxs-lookup"><span data-stu-id="f4600-112">Actually, MySQL5.6 has more improvement in performance than MySQL5.5.</span></span>  <span data-ttu-id="f4600-113">Mer information [här](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span><span class="sxs-lookup"><span data-stu-id="f4600-113">More information [here](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span></span>

### <a name="how-tooinstall-mysql56-on-ubuntu"></a><span data-ttu-id="f4600-114">Hur tooinstall MySQL5.6 på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="f4600-114">How tooinstall MySQL5.6 on Ubuntu</span></span>
<span data-ttu-id="f4600-115">Vi använder Linux VM med Ubuntu från Azure här.</span><span class="sxs-lookup"><span data-stu-id="f4600-115">We will use Linux VM with Ubuntu from Azure here.</span></span>

* <span data-ttu-id="f4600-116">Steg 1: Installera MySQL Server 5.6 växla för`root` användare:</span><span class="sxs-lookup"><span data-stu-id="f4600-116">Step 1: Install MySQL Server 5.6   Switch too`root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="f4600-117">Installera server 5.6 mysql:</span><span class="sxs-lookup"><span data-stu-id="f4600-117">Install mysql-server 5.6:</span></span>
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    <span data-ttu-id="f4600-118">Under installationen visas en dialogruta fönstret poping in tooask du tooset MySQL rotlösenordet nedan, och du måste ange hello lösenord här.</span><span class="sxs-lookup"><span data-stu-id="f4600-118">During installation, you will see a dialog window poping up tooask you tooset MySQL root password below, and you need set hello password here.</span></span>
  
    ![Bild](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    <span data-ttu-id="f4600-120">Inkommande hello lösenord igen tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="f4600-120">Input hello password again tooconfirm.</span></span>

    ![Bild](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* <span data-ttu-id="f4600-122">Steg 2: Logga in MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="f4600-122">Step 2: Login MySQL Server</span></span>
  
    <span data-ttu-id="f4600-123">När du är klar MySQL-serverinstallation startas MySQL-tjänsten automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f4600-123">When MySQL server installation finished, MySQL service will be started automatically.</span></span> <span data-ttu-id="f4600-124">Du kan logga in MySQL-Server med `root` användare.</span><span class="sxs-lookup"><span data-stu-id="f4600-124">You can login MySQL Server with `root` user.</span></span>
    <span data-ttu-id="f4600-125">Använd hello nedan kommandot toologin och indata lösenord.</span><span class="sxs-lookup"><span data-stu-id="f4600-125">Use hello below command toologin and input password.</span></span>
  
             #[root@mysqlnode ~]# mysql -uroot -p
* <span data-ttu-id="f4600-126">Steg 3: Hantera hello MySQL-tjänsten körs</span><span class="sxs-lookup"><span data-stu-id="f4600-126">Step 3: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="f4600-127">(a) Hämta status för MySQL-tjänst</span><span class="sxs-lookup"><span data-stu-id="f4600-127">(a) Get status of MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql status
  
    <span data-ttu-id="f4600-128">(b) starta MySQL-tjänst</span><span class="sxs-lookup"><span data-stu-id="f4600-128">(b) Start MySQL Service</span></span>
  
             #[root@mysqlnode ~]# service mysql start
  
    <span data-ttu-id="f4600-129">(c) Stoppa MySQL-tjänst</span><span class="sxs-lookup"><span data-stu-id="f4600-129">(c) Stop MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql stop
  
    <span data-ttu-id="f4600-130">(d) starta om hello MySQL-tjänst</span><span class="sxs-lookup"><span data-stu-id="f4600-130">(d) Restart hello MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-tooinstall-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a><span data-ttu-id="f4600-131">Hur tooinstall MySQL för Red Hat OS-familjen som CentOS, Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="f4600-131">How tooinstall MySQL on Red Hat OS family like CentOS, Oracle Linux</span></span>
<span data-ttu-id="f4600-132">Vi använder Linux VM med CentOS eller Oracle Linux här.</span><span class="sxs-lookup"><span data-stu-id="f4600-132">We will use Linux VM with CentOS or Oracle Linux here.</span></span>

* <span data-ttu-id="f4600-133">Steg 1: Lägg till hello MySQL Yum databasen växeln för`root` användare:</span><span class="sxs-lookup"><span data-stu-id="f4600-133">Step 1: Add hello MySQL Yum repository   Switch too`root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="f4600-134">Hämta och installera hello MySQL versionspaket:</span><span class="sxs-lookup"><span data-stu-id="f4600-134">Download and install hello MySQL release package:</span></span>
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* <span data-ttu-id="f4600-135">Steg 2: Redigera nedan filen tooenable hello MySQL-databasen för att ladda ned hello MySQL5.6 paketet.</span><span class="sxs-lookup"><span data-stu-id="f4600-135">Step 2: Edit below file tooenable hello MySQL repository for downloading hello MySQL5.6 package.</span></span>
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    <span data-ttu-id="f4600-136">Uppdatera varje värde i den här filen toobelow:</span><span class="sxs-lookup"><span data-stu-id="f4600-136">Update each value of this file toobelow:</span></span>
  
        \# *Enable toouse MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* <span data-ttu-id="f4600-137">Steg 3: Installera MySQL från MySQL-databas installera MySQL:</span><span class="sxs-lookup"><span data-stu-id="f4600-137">Step 3: Install MySQL from MySQL repository   Install MySQL:</span></span>
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    <span data-ttu-id="f4600-138">MySQL-RPM-paket och alla relaterade paket kommer att installeras.</span><span class="sxs-lookup"><span data-stu-id="f4600-138">MySQL RPM package and all related packages will be installed.</span></span>
* <span data-ttu-id="f4600-139">Steg 4: Hantera hello MySQL-tjänsten körs</span><span class="sxs-lookup"><span data-stu-id="f4600-139">Step 4: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="f4600-140">(a) kontrollera hello tjänstestatus för hello MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="f4600-140">(a) Check hello service status of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]#service mysqld status
  
    <span data-ttu-id="f4600-141">(b) kontrollera om hello standard port MySQL-servern körs:</span><span class="sxs-lookup"><span data-stu-id="f4600-141">(b) Check whether hello default port of  MySQL server is running:</span></span>
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    <span data-ttu-id="f4600-142">(c) starta hello MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="f4600-142">(c) Start hello MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld start

    <span data-ttu-id="f4600-143">(d) Stoppa hello MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="f4600-143">(d) Stop hello MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld stop

    <span data-ttu-id="f4600-144">(e) Ange MySQL toostart när hello system uppstart:</span><span class="sxs-lookup"><span data-stu-id="f4600-144">(e) Set MySQL toostart when hello system boot-up:</span></span>

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-tooinstall-mysql-on-suse-linux"></a><span data-ttu-id="f4600-145">Hur tooinstall MySQL på SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="f4600-145">How tooinstall MySQL on SUSE Linux</span></span>
<span data-ttu-id="f4600-146">Vi använder Linux VM med OpenSUSE här.</span><span class="sxs-lookup"><span data-stu-id="f4600-146">We will use Linux VM with OpenSUSE here.</span></span>

* <span data-ttu-id="f4600-147">Steg 1: Hämta och installera MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="f4600-147">Step 1: Download and install MySQL Server</span></span>
  
    <span data-ttu-id="f4600-148">Växla för`root` användare via nedan kommando:</span><span class="sxs-lookup"><span data-stu-id="f4600-148">Switch too`root` user through below command:</span></span>  
  
           #sudo su -
  
    <span data-ttu-id="f4600-149">Hämta och installera MySQL-paketet:</span><span class="sxs-lookup"><span data-stu-id="f4600-149">Download and install MySQL package:</span></span>
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* <span data-ttu-id="f4600-150">Steg 2: Hantera hello MySQL-tjänsten körs</span><span class="sxs-lookup"><span data-stu-id="f4600-150">Step 2: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="f4600-151">(a) kontrollera hello statusen för hello MySQL-servern:</span><span class="sxs-lookup"><span data-stu-id="f4600-151">(a) Check hello status of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]# rcmysql status
  
    <span data-ttu-id="f4600-152">(b) kontrollera om hello standardporten hello MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="f4600-152">(b) Check whether hello default port of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    <span data-ttu-id="f4600-153">(c) starta hello MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="f4600-153">(c) Start hello MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql start

    <span data-ttu-id="f4600-154">(d) Stoppa hello MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="f4600-154">(d) Stop hello MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql stop

    <span data-ttu-id="f4600-155">(e) Ange MySQL toostart när hello system uppstart:</span><span class="sxs-lookup"><span data-stu-id="f4600-155">(e) Set MySQL toostart when hello system boot-up:</span></span>

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a><span data-ttu-id="f4600-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f4600-156">Next Step</span></span>
<span data-ttu-id="f4600-157">Mer information om MySQL och användning finns [här](https://www.mysql.com/).</span><span class="sxs-lookup"><span data-stu-id="f4600-157">Find more usage and information regarding MySQL [Here](https://www.mysql.com/).</span></span>

