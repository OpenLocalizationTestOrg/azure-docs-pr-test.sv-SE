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
# <a name="how-tooinstall-mysql-on-azure"></a>Hur tooinstall MySQL på Azure
I den här artikeln får du lära dig hur tooinstall och konfigurera MySQL på en Azure-dator som kör Linux.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>Installera MySQL på den virtuella datorn
> [!NOTE]
> Du måste redan ha en Microsoft Azure-dator som kör Linux i ordning toocomplete den här kursen. Finns det [virtuella Azure Linux-datorn kursen](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate och konfigurera en Linux-VM med `mysqlnode` som hello VM namn och `azureuser` som användare innan du fortsätter.
> 
> 

I det här fallet Använd 3306 port som hello MySQL-port.  

Ansluta toohello Linux VM du skapat via putty. Hello första gången du använder virtuella Azure Linux-datorn, se hur toouse putty ansluter tooa Linux VM [här](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Vi använder databasen paketet tooinstall MySQL5.6 som exempel i den här artikeln. Faktiskt har MySQL5.6 flera förbättringar i prestanda än MySQL5.5.  Mer information [här](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-tooinstall-mysql56-on-ubuntu"></a>Hur tooinstall MySQL5.6 på Ubuntu
Vi använder Linux VM med Ubuntu från Azure här.

* Steg 1: Installera MySQL Server 5.6 växla för`root` användare:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Installera server 5.6 mysql:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Under installationen visas en dialogruta fönstret poping in tooask du tooset MySQL rotlösenordet nedan, och du måste ange hello lösenord här.
  
    ![Bild](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Inkommande hello lösenord igen tooconfirm.

    ![Bild](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* Steg 2: Logga in MySQL-Server
  
    När du är klar MySQL-serverinstallation startas MySQL-tjänsten automatiskt. Du kan logga in MySQL-Server med `root` användare.
    Använd hello nedan kommandot toologin och indata lösenord.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* Steg 3: Hantera hello MySQL-tjänsten körs
  
    (a) Hämta status för MySQL-tjänst
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) starta MySQL-tjänst
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) Stoppa MySQL-tjänst
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) starta om hello MySQL-tjänst
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-tooinstall-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Hur tooinstall MySQL för Red Hat OS-familjen som CentOS, Oracle Linux
Vi använder Linux VM med CentOS eller Oracle Linux här.

* Steg 1: Lägg till hello MySQL Yum databasen växeln för`root` användare:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Hämta och installera hello MySQL versionspaket:
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* Steg 2: Redigera nedan filen tooenable hello MySQL-databasen för att ladda ned hello MySQL5.6 paketet.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Uppdatera varje värde i den här filen toobelow:
  
        \# *Enable toouse MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* Steg 3: Installera MySQL från MySQL-databas installera MySQL:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    MySQL-RPM-paket och alla relaterade paket kommer att installeras.
* Steg 4: Hantera hello MySQL-tjänsten körs
  
    (a) kontrollera hello tjänstestatus för hello MySQL-server:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) kontrollera om hello standard port MySQL-servern körs:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) starta hello MySQL-server:

           #[root@mysqlnode ~]#service mysqld start

    (d) Stoppa hello MySQL-server:

           #[root@mysqlnode ~]#service mysqld stop

    (e) Ange MySQL toostart när hello system uppstart:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-tooinstall-mysql-on-suse-linux"></a>Hur tooinstall MySQL på SUSE Linux
Vi använder Linux VM med OpenSUSE här.

* Steg 1: Hämta och installera MySQL-Server
  
    Växla för`root` användare via nedan kommando:  
  
           #sudo su -
  
    Hämta och installera MySQL-paketet:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* Steg 2: Hantera hello MySQL-tjänsten körs
  
    (a) kontrollera hello statusen för hello MySQL-servern:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) kontrollera om hello standardporten hello MySQL-server:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) starta hello MySQL-server:

           #[root@mysqlnode ~]# rcmysql start

    (d) Stoppa hello MySQL-server:

           #[root@mysqlnode ~]# rcmysql stop

    (e) Ange MySQL toostart när hello system uppstart:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Nästa steg
Mer information om MySQL och användning finns [här](https://www.mysql.com/).

