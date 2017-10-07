---
title: "inställningar för aaaAzure N-serien drivrutin för Linux | Microsoft Docs"
description: "Hur tooset in NVIDIA GPU drivrutiner för N-serien virtuella datorer som kör Linux i Azure"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7db1b3859f9075c6d9f0319f39418946ea08743f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Installera drivrutiner för NVIDIA GPU på N-serien virtuella datorer som kör Linux

tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Linux, installera stöd för grafik drivrutinerna. Den här artikeln innehåller drivrutinen konfigurationsstegen när du distribuerar en virtuell dator i N-serien. Inställningsinformation för drivrutinen är också tillgängligt för [virtuella Windows-datorer](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


N-serien VM specifikationerna, lagringskapacitet, och diskinformation finns [GPU Linux VM-storlekar](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a>Installera drivrutiner för rutnät för NV virtuella datorer

tooinstall NVIDIA RUTNÄTET drivrutiner på NV virtuella datorer, se en SSH-anslutning tooeach VM och följ hello stegen för Linux-distribution. 

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Kör hello `lspci` kommando. Kontrollera att hello NVIDIA M60 kort eller kort visas som PCI-enheter.

2. Installera uppdateringar.

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. Inaktivera hello Nouveau kernel-drivrutinen, som inte är kompatibel med hello NVIDIA drivrutinen. (Endast använda hello NVIDIA drivrutinen på NV virtuella datorer.) toodo, skapa en fil i `/etc/modprobe.d `med namnet `nouveau.conf` med hello följande innehåll:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. Starta om hello VM och återansluta. Avsluta X-server:

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. Hämta och installera drivrutinen för hello RUTNÄTET:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. När du tillfrågas om du vill toorun hello nvidia xconfig verktyget tooupdate konfigurationsfilen X, Välj **Ja**.

7. När installationen är klar kopiera /etc/nvidia/gridd.conf.template tooa nya filen gridd.conf på plats/etc/nvidia /

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. Lägg till följande hello för`/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Starta om hello VM och fortsätta tooverify hello installation.


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS-baserade 7.3 eller Red Hat Enterprise Linux 7.3


1. Uppdatera hello kernel och DKMS.
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. Inaktivera hello Nouveau kernel-drivrutinen, som inte är kompatibel med hello NVIDIA drivrutinen. (Endast använda hello NVIDIA drivrutinen på NV virtuella datorer.) toodo, skapa en fil i `/etc/modprobe.d `med namnet `nouveau.conf` med hello följande innehåll:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. Starta om hello VM, återansluta och installera hello senaste Linux integreringstjänsterna för Hyper-V:
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. Anslut toohello VM och kör hello `lspci` kommando. Kontrollera att hello NVIDIA M60 kort eller kort visas som PCI-enheter.
 
5. Hämta och installera drivrutinen för hello RUTNÄTET:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. När du tillfrågas om du vill toorun hello nvidia xconfig verktyget tooupdate konfigurationsfilen X, Välj **Ja**.

7. När installationen är klar kopiera /etc/nvidia/gridd.conf.template tooa nya filen gridd.conf på plats/etc/nvidia /
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. Lägg till följande hello för`/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Starta om hello VM och fortsätta tooverify hello installation.

### <a name="verify-driver-installation"></a>Verifiera installation av drivrutiner


tooquery hello GPU enhetstillstånd SSH toohello VM och kör hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) kommandoradsverktyg som installerats med hello-drivrutinen. 

Utdata liknande toohello följande visas:

![NVIDIA Enhetsstatus](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 server
Om du behöver en X11 server för fjärranslutningar tooan NV VM [x11vnc](http://www.karlrunge.com/x11vnc/) rekommenderas eftersom det tillåter maskinvaruacceleration för grafik. Hej BusID av hello M60 enhet måste läggas till manuellt toohello xconfig fil (`etc/X11/xorg.conf` på Ubuntu 16.04 LTS `/etc/X11/XF86config` på CentOS 7.3 eller Red Hat Enterprise Server 7.3). Lägg till en `"Device"` avsnittet liknande toohello följande:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
Dessutom uppdatera din `"Screen"` avsnittet toouse den här enheten.
 
Hej BusID hittar du genom att köra

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
Hej BusID kan ändras när en virtuell dator hämtar omfördelats startas om. Därför kanske toouse ett skript tooupdate hello BusID i hello X11 konfiguration när en virtuell dator startas. Exempel:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed too${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Den här filen kan anropas som rot på Start genom att skapa en post för den i `/etc/rc.d/rc3.d`.


## <a name="install-cuda-drivers-for-nc-vms"></a>Installera CUDA drivrutiner för NC virtuella datorer

Här följer stegen tooinstall drivrutinerna på Linux NC virtuella datorer från hello NVIDIA CUDA Toolkit 8.0. 

C och C++ utvecklare kan välja att installera hello fullständiga Toolkit toobuild GPU-snabbare program. Mer information finns i hello [CUDA installationsguiden](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).


> [!NOTE]
> CUDA drivrutinen länkar som anges här aktuella vid tiden för publikationen. Hello senaste CUDA drivrutinerna finns hello [NVIDIA](http://www.nvidia.com/) webbplats.
>

tooinstall CUDA Toolkit gör en SSH-anslutning tooeach VM. tooverify som hello system har en CUDA-kompatibla GPU kör hello följande kommando:

```bash
lspci | grep -i NVIDIA
```
Du kommer se utdata liknande toohello följande exempel (visar ett NVIDIA Tesla K80-kort):

![lspci kommandoutdata](./media/n-series-driver-setup/lspci.png)

Sedan kör installationskommandon som är specifika för din distribution.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Hämta och installera hello CUDA drivrutiner.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  hello-installationen kan ta flera minuter.

2. toooptionally installera hello komplett CUDA verktyg, typ:

  ```bash
  sudo apt-get install cuda
  ```

3. Starta om hello VM och fortsätta tooverify hello installation.

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS-baserade 7.3 eller Red Hat Enterprise Linux 7.3

1. Hämta uppdateringar. 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. Anslut toohello VM och installera hello senaste Linux integreringstjänsterna för Hyper-V.

  > [!IMPORTANT]
  > Om du har installerat en CentOS-baserade HPC-avbildning på en virtuell dator NC24r hoppar du över tooStep 3. Eftersom Azure RDMA drivrutiner och Linux-integreringstjänster installeras före hello bild, LIS uppgraderas inte och kernel-uppdateringar är inaktiverad som standard.
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. Anslut toohello VM och fortsätta installationen med hello följande kommandon:

  ```bash
  sudo yum install kernel-devel

  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  sudo yum install dkms

  CUDA_REPO_PKG=cuda-repo-rhel7-8.0.61-1.x86_64.rpm

  wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

  sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo yum install cuda-drivers
  ```

  hello-installationen kan ta flera minuter. 

4. toooptionally installera hello komplett CUDA verktyg, typ:

  ```bash
  sudo yum install cuda
  ```

5. Starta om hello VM och fortsätta tooverify hello installation.


### <a name="verify-driver-installation"></a>Verifiera installation av drivrutiner


tooquery hello GPU enhetstillstånd SSH toohello VM och kör hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) kommandoradsverktyg som installerats med hello-drivrutinen. 

Utdata liknande toohello följande visas:

![NVIDIA Enhetsstatus](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a>CUDA drivrutinsuppdateringar

Vi rekommenderar att du regelbundet uppdatera CUDA drivrutiner efter distributionen.

#### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS-baserade 7.3 eller Red Hat Enterprise Linux 7.3

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a>Felsökning

* Det finns ett känt problem med CUDA drivrutiner på Azure N-serien virtuella datorer körs hello 4.4.0-75 Linux kernel på Ubuntu 16.04 LTS. Om du uppgraderar från en tidigare version av kernel uppgradera tooat minst 4.4.0-77 för kernel-version. 



## <a name="next-steps"></a>Nästa steg

* Mer information om hello NVIDIA GPU-kort på hello VMs N-serien finns i:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (för Azure NC virtuella datorer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (för Azure NV virtuella datorer)

* toocapture en Linux-VM-avbildning med din installerade drivrutinerna finns [hur toogeneralize och avbilda en virtuell Linux-dator](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
