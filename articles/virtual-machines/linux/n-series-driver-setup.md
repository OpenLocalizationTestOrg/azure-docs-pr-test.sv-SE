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
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a><span data-ttu-id="09fc2-103">Installera drivrutiner för NVIDIA GPU på N-serien virtuella datorer som kör Linux</span><span class="sxs-lookup"><span data-stu-id="09fc2-103">Install NVIDIA GPU drivers on N-series VMs running Linux</span></span>

<span data-ttu-id="09fc2-104">tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Linux, installera stöd för grafik drivrutinerna.</span><span class="sxs-lookup"><span data-stu-id="09fc2-104">tootake advantage of hello GPU capabilities of Azure N-series VMs running Linux, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="09fc2-105">Den här artikeln innehåller drivrutinen konfigurationsstegen när du distribuerar en virtuell dator i N-serien.</span><span class="sxs-lookup"><span data-stu-id="09fc2-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="09fc2-106">Inställningsinformation för drivrutinen är också tillgängligt för [virtuella Windows-datorer](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="09fc2-106">Driver setup information is also available for [Windows VMs](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


<span data-ttu-id="09fc2-107">N-serien VM specifikationerna, lagringskapacitet, och diskinformation finns [GPU Linux VM-storlekar](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="09fc2-107">For N-series VM specs, storage capacities, and disk details, see [GPU Linux VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a><span data-ttu-id="09fc2-108">Installera drivrutiner för rutnät för NV virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="09fc2-108">Install GRID drivers for NV VMs</span></span>

<span data-ttu-id="09fc2-109">tooinstall NVIDIA RUTNÄTET drivrutiner på NV virtuella datorer, se en SSH-anslutning tooeach VM och följ hello stegen för Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="09fc2-109">tooinstall NVIDIA GRID drivers on NV VMs, make an SSH connection tooeach VM and follow hello steps for your Linux distribution.</span></span> 

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="09fc2-110">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="09fc2-110">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="09fc2-111">Kör hello `lspci` kommando.</span><span class="sxs-lookup"><span data-stu-id="09fc2-111">Run hello `lspci` command.</span></span> <span data-ttu-id="09fc2-112">Kontrollera att hello NVIDIA M60 kort eller kort visas som PCI-enheter.</span><span class="sxs-lookup"><span data-stu-id="09fc2-112">Verify that hello NVIDIA M60 card or cards are visible as PCI devices.</span></span>

2. <span data-ttu-id="09fc2-113">Installera uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="09fc2-113">Install updates.</span></span>

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. <span data-ttu-id="09fc2-114">Inaktivera hello Nouveau kernel-drivrutinen, som inte är kompatibel med hello NVIDIA drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="09fc2-114">Disable hello Nouveau kernel driver, which is incompatible with hello NVIDIA driver.</span></span> <span data-ttu-id="09fc2-115">(Endast använda hello NVIDIA drivrutinen på NV virtuella datorer.) toodo, skapa en fil i `/etc/modprobe.d `med namnet `nouveau.conf` med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="09fc2-115">(Only use hello NVIDIA driver on NV VMs.) toodo this, create a file in `/etc/modprobe.d `named `nouveau.conf` with hello following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. <span data-ttu-id="09fc2-116">Starta om hello VM och återansluta.</span><span class="sxs-lookup"><span data-stu-id="09fc2-116">Reboot hello VM and reconnect.</span></span> <span data-ttu-id="09fc2-117">Avsluta X-server:</span><span class="sxs-lookup"><span data-stu-id="09fc2-117">Exit X server:</span></span>

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. <span data-ttu-id="09fc2-118">Hämta och installera drivrutinen för hello RUTNÄTET:</span><span class="sxs-lookup"><span data-stu-id="09fc2-118">Download and install hello GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. <span data-ttu-id="09fc2-119">När du tillfrågas om du vill toorun hello nvidia xconfig verktyget tooupdate konfigurationsfilen X, Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="09fc2-119">When you're asked whether you want toorun hello nvidia-xconfig utility tooupdate your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="09fc2-120">När installationen är klar kopiera /etc/nvidia/gridd.conf.template tooa nya filen gridd.conf på plats/etc/nvidia /</span><span class="sxs-lookup"><span data-stu-id="09fc2-120">After installation completes, copy /etc/nvidia/gridd.conf.template tooa new file gridd.conf at location /etc/nvidia/</span></span>

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. <span data-ttu-id="09fc2-121">Lägg till följande hello för`/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="09fc2-121">Add hello following too`/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="09fc2-122">Starta om hello VM och fortsätta tooverify hello installation.</span><span class="sxs-lookup"><span data-stu-id="09fc2-122">Reboot hello VM and proceed tooverify hello installation.</span></span>


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="09fc2-123">CentOS-baserade 7.3 eller Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="09fc2-123">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>


1. <span data-ttu-id="09fc2-124">Uppdatera hello kernel och DKMS.</span><span class="sxs-lookup"><span data-stu-id="09fc2-124">Update hello kernel and DKMS.</span></span>
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. <span data-ttu-id="09fc2-125">Inaktivera hello Nouveau kernel-drivrutinen, som inte är kompatibel med hello NVIDIA drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="09fc2-125">Disable hello Nouveau kernel driver, which is incompatible with hello NVIDIA driver.</span></span> <span data-ttu-id="09fc2-126">(Endast använda hello NVIDIA drivrutinen på NV virtuella datorer.) toodo, skapa en fil i `/etc/modprobe.d `med namnet `nouveau.conf` med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="09fc2-126">(Only use hello NVIDIA driver on NV VMs.) toodo this, create a file in `/etc/modprobe.d `named `nouveau.conf` with hello following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. <span data-ttu-id="09fc2-127">Starta om hello VM, återansluta och installera hello senaste Linux integreringstjänsterna för Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="09fc2-127">Reboot hello VM, reconnect, and install hello latest Linux Integration Services for Hyper-V:</span></span>
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. <span data-ttu-id="09fc2-128">Anslut toohello VM och kör hello `lspci` kommando.</span><span class="sxs-lookup"><span data-stu-id="09fc2-128">Reconnect toohello VM and run hello `lspci` command.</span></span> <span data-ttu-id="09fc2-129">Kontrollera att hello NVIDIA M60 kort eller kort visas som PCI-enheter.</span><span class="sxs-lookup"><span data-stu-id="09fc2-129">Verify that hello NVIDIA M60 card or cards are visible as PCI devices.</span></span>
 
5. <span data-ttu-id="09fc2-130">Hämta och installera drivrutinen för hello RUTNÄTET:</span><span class="sxs-lookup"><span data-stu-id="09fc2-130">Download and install hello GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. <span data-ttu-id="09fc2-131">När du tillfrågas om du vill toorun hello nvidia xconfig verktyget tooupdate konfigurationsfilen X, Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="09fc2-131">When you're asked whether you want toorun hello nvidia-xconfig utility tooupdate your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="09fc2-132">När installationen är klar kopiera /etc/nvidia/gridd.conf.template tooa nya filen gridd.conf på plats/etc/nvidia /</span><span class="sxs-lookup"><span data-stu-id="09fc2-132">After installation completes, copy /etc/nvidia/gridd.conf.template tooa new file gridd.conf at location /etc/nvidia/</span></span>
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. <span data-ttu-id="09fc2-133">Lägg till följande hello för`/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="09fc2-133">Add hello following too`/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="09fc2-134">Starta om hello VM och fortsätta tooverify hello installation.</span><span class="sxs-lookup"><span data-stu-id="09fc2-134">Reboot hello VM and proceed tooverify hello installation.</span></span>

### <a name="verify-driver-installation"></a><span data-ttu-id="09fc2-135">Verifiera installation av drivrutiner</span><span class="sxs-lookup"><span data-stu-id="09fc2-135">Verify driver installation</span></span>


<span data-ttu-id="09fc2-136">tooquery hello GPU enhetstillstånd SSH toohello VM och kör hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) kommandoradsverktyg som installerats med hello-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="09fc2-136">tooquery hello GPU device state, SSH toohello VM and run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span> 

<span data-ttu-id="09fc2-137">Utdata liknande toohello följande visas:</span><span class="sxs-lookup"><span data-stu-id="09fc2-137">Output similar toohello following appears:</span></span>

![NVIDIA Enhetsstatus](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a><span data-ttu-id="09fc2-139">X11 server</span><span class="sxs-lookup"><span data-stu-id="09fc2-139">X11 server</span></span>
<span data-ttu-id="09fc2-140">Om du behöver en X11 server för fjärranslutningar tooan NV VM [x11vnc](http://www.karlrunge.com/x11vnc/) rekommenderas eftersom det tillåter maskinvaruacceleration för grafik.</span><span class="sxs-lookup"><span data-stu-id="09fc2-140">If you need an X11 server for remote connections tooan NV VM, [x11vnc](http://www.karlrunge.com/x11vnc/) is recommended because it allows hardware acceleration of graphics.</span></span> <span data-ttu-id="09fc2-141">Hej BusID av hello M60 enhet måste läggas till manuellt toohello xconfig fil (`etc/X11/xorg.conf` på Ubuntu 16.04 LTS `/etc/X11/XF86config` på CentOS 7.3 eller Red Hat Enterprise Server 7.3).</span><span class="sxs-lookup"><span data-stu-id="09fc2-141">hello BusID of hello M60 device must be manually added toohello xconfig file (`etc/X11/xorg.conf` on Ubuntu 16.04 LTS, `/etc/X11/XF86config` on CentOS 7.3 or Red Hat Enterprise Server 7.3).</span></span> <span data-ttu-id="09fc2-142">Lägg till en `"Device"` avsnittet liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="09fc2-142">Add a `"Device"` section similar toohello following:</span></span>
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
<span data-ttu-id="09fc2-143">Dessutom uppdatera din `"Screen"` avsnittet toouse den här enheten.</span><span class="sxs-lookup"><span data-stu-id="09fc2-143">Additionally, update your `"Screen"` section toouse this device.</span></span>
 
<span data-ttu-id="09fc2-144">Hej BusID hittar du genom att köra</span><span class="sxs-lookup"><span data-stu-id="09fc2-144">hello BusID can be found by running</span></span>

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
<span data-ttu-id="09fc2-145">Hej BusID kan ändras när en virtuell dator hämtar omfördelats startas om.</span><span class="sxs-lookup"><span data-stu-id="09fc2-145">hello BusID can change when a VM gets reallocated or rebooted.</span></span> <span data-ttu-id="09fc2-146">Därför kanske toouse ett skript tooupdate hello BusID i hello X11 konfiguration när en virtuell dator startas.</span><span class="sxs-lookup"><span data-stu-id="09fc2-146">Therefore, you may want toouse a script tooupdate hello BusID in hello X11 configuration when a VM is rebooted.</span></span> <span data-ttu-id="09fc2-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="09fc2-147">For example:</span></span>

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed too${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

<span data-ttu-id="09fc2-148">Den här filen kan anropas som rot på Start genom att skapa en post för den i `/etc/rc.d/rc3.d`.</span><span class="sxs-lookup"><span data-stu-id="09fc2-148">This file can be invoked as root on boot by creating an entry for it in `/etc/rc.d/rc3.d`.</span></span>


## <a name="install-cuda-drivers-for-nc-vms"></a><span data-ttu-id="09fc2-149">Installera CUDA drivrutiner för NC virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="09fc2-149">Install CUDA drivers for NC VMs</span></span>

<span data-ttu-id="09fc2-150">Här följer stegen tooinstall drivrutinerna på Linux NC virtuella datorer från hello NVIDIA CUDA Toolkit 8.0.</span><span class="sxs-lookup"><span data-stu-id="09fc2-150">Here are steps tooinstall NVIDIA drivers on Linux NC VMs from hello NVIDIA CUDA Toolkit 8.0.</span></span> 

<span data-ttu-id="09fc2-151">C och C++ utvecklare kan välja att installera hello fullständiga Toolkit toobuild GPU-snabbare program.</span><span class="sxs-lookup"><span data-stu-id="09fc2-151">C and C++ developers can optionally install hello full Toolkit toobuild GPU-accelerated applications.</span></span> <span data-ttu-id="09fc2-152">Mer information finns i hello [CUDA installationsguiden](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span><span class="sxs-lookup"><span data-stu-id="09fc2-152">For more information, see hello [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span></span>


> [!NOTE]
> <span data-ttu-id="09fc2-153">CUDA drivrutinen länkar som anges här aktuella vid tiden för publikationen.</span><span class="sxs-lookup"><span data-stu-id="09fc2-153">CUDA driver download links provided here are current at time of publication.</span></span> <span data-ttu-id="09fc2-154">Hello senaste CUDA drivrutinerna finns hello [NVIDIA](http://www.nvidia.com/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="09fc2-154">For hello latest CUDA drivers, visit hello [NVIDIA](http://www.nvidia.com/) website.</span></span>
>

<span data-ttu-id="09fc2-155">tooinstall CUDA Toolkit gör en SSH-anslutning tooeach VM.</span><span class="sxs-lookup"><span data-stu-id="09fc2-155">tooinstall CUDA Toolkit, make an SSH connection tooeach VM.</span></span> <span data-ttu-id="09fc2-156">tooverify som hello system har en CUDA-kompatibla GPU kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="09fc2-156">tooverify that hello system has a CUDA-capable GPU, run hello following command:</span></span>

```bash
lspci | grep -i NVIDIA
```
<span data-ttu-id="09fc2-157">Du kommer se utdata liknande toohello följande exempel (visar ett NVIDIA Tesla K80-kort):</span><span class="sxs-lookup"><span data-stu-id="09fc2-157">You will see output similar toohello following example (showing an NVIDIA Tesla K80 card):</span></span>

![lspci kommandoutdata](./media/n-series-driver-setup/lspci.png)

<span data-ttu-id="09fc2-159">Sedan kör installationskommandon som är specifika för din distribution.</span><span class="sxs-lookup"><span data-stu-id="09fc2-159">Then run installation commands specific for your distribution.</span></span>

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="09fc2-160">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="09fc2-160">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="09fc2-161">Hämta och installera hello CUDA drivrutiner.</span><span class="sxs-lookup"><span data-stu-id="09fc2-161">Download and install hello CUDA drivers.</span></span>
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  <span data-ttu-id="09fc2-162">hello-installationen kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="09fc2-162">hello installation can take several minutes.</span></span>

2. <span data-ttu-id="09fc2-163">toooptionally installera hello komplett CUDA verktyg, typ:</span><span class="sxs-lookup"><span data-stu-id="09fc2-163">toooptionally install hello complete CUDA toolkit, type:</span></span>

  ```bash
  sudo apt-get install cuda
  ```

3. <span data-ttu-id="09fc2-164">Starta om hello VM och fortsätta tooverify hello installation.</span><span class="sxs-lookup"><span data-stu-id="09fc2-164">Reboot hello VM and proceed tooverify hello installation.</span></span>

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="09fc2-165">CentOS-baserade 7.3 eller Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="09fc2-165">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

1. <span data-ttu-id="09fc2-166">Hämta uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="09fc2-166">Get updates.</span></span> 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. <span data-ttu-id="09fc2-167">Anslut toohello VM och installera hello senaste Linux integreringstjänsterna för Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="09fc2-167">Reconnect toohello VM and install hello latest Linux Integration Services for Hyper-V.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="09fc2-168">Om du har installerat en CentOS-baserade HPC-avbildning på en virtuell dator NC24r hoppar du över tooStep 3.</span><span class="sxs-lookup"><span data-stu-id="09fc2-168">If you installed a CentOS-based HPC image on an NC24r VM, skip tooStep 3.</span></span> <span data-ttu-id="09fc2-169">Eftersom Azure RDMA drivrutiner och Linux-integreringstjänster installeras före hello bild, LIS uppgraderas inte och kernel-uppdateringar är inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="09fc2-169">Because Azure RDMA drivers and Linux Integration Services are pre-installed in hello image, LIS should not be upgraded, and kernel updates are disabled by default.</span></span>
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. <span data-ttu-id="09fc2-170">Anslut toohello VM och fortsätta installationen med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="09fc2-170">Reconnect toohello VM and continue installation with hello following commands:</span></span>

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

  <span data-ttu-id="09fc2-171">hello-installationen kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="09fc2-171">hello installation can take several minutes.</span></span> 

4. <span data-ttu-id="09fc2-172">toooptionally installera hello komplett CUDA verktyg, typ:</span><span class="sxs-lookup"><span data-stu-id="09fc2-172">toooptionally install hello complete CUDA toolkit, type:</span></span>

  ```bash
  sudo yum install cuda
  ```

5. <span data-ttu-id="09fc2-173">Starta om hello VM och fortsätta tooverify hello installation.</span><span class="sxs-lookup"><span data-stu-id="09fc2-173">Reboot hello VM and proceed tooverify hello installation.</span></span>


### <a name="verify-driver-installation"></a><span data-ttu-id="09fc2-174">Verifiera installation av drivrutiner</span><span class="sxs-lookup"><span data-stu-id="09fc2-174">Verify driver installation</span></span>


<span data-ttu-id="09fc2-175">tooquery hello GPU enhetstillstånd SSH toohello VM och kör hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) kommandoradsverktyg som installerats med hello-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="09fc2-175">tooquery hello GPU device state, SSH toohello VM and run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span> 

<span data-ttu-id="09fc2-176">Utdata liknande toohello följande visas:</span><span class="sxs-lookup"><span data-stu-id="09fc2-176">Output similar toohello following appears:</span></span>

![NVIDIA Enhetsstatus](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a><span data-ttu-id="09fc2-178">CUDA drivrutinsuppdateringar</span><span class="sxs-lookup"><span data-stu-id="09fc2-178">CUDA driver updates</span></span>

<span data-ttu-id="09fc2-179">Vi rekommenderar att du regelbundet uppdatera CUDA drivrutiner efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="09fc2-179">We recommend that you periodically update CUDA drivers after deployment.</span></span>

#### <a name="ubuntu-1604-lts"></a><span data-ttu-id="09fc2-180">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="09fc2-180">Ubuntu 16.04 LTS</span></span>

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="09fc2-181">CentOS-baserade 7.3 eller Red Hat Enterprise Linux 7.3</span><span class="sxs-lookup"><span data-stu-id="09fc2-181">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a><span data-ttu-id="09fc2-182">Felsökning</span><span class="sxs-lookup"><span data-stu-id="09fc2-182">Troubleshooting</span></span>

* <span data-ttu-id="09fc2-183">Det finns ett känt problem med CUDA drivrutiner på Azure N-serien virtuella datorer körs hello 4.4.0-75 Linux kernel på Ubuntu 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="09fc2-183">There is a known issue with CUDA drivers on Azure N-series VMs running hello 4.4.0-75 Linux kernel on Ubuntu 16.04 LTS.</span></span> <span data-ttu-id="09fc2-184">Om du uppgraderar från en tidigare version av kernel uppgradera tooat minst 4.4.0-77 för kernel-version.</span><span class="sxs-lookup"><span data-stu-id="09fc2-184">If you are upgrading from an earlier kernel version, upgrade tooat least kernel version 4.4.0-77.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="09fc2-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="09fc2-185">Next steps</span></span>

* <span data-ttu-id="09fc2-186">Mer information om hello NVIDIA GPU-kort på hello VMs N-serien finns i:</span><span class="sxs-lookup"><span data-stu-id="09fc2-186">For more information about hello NVIDIA GPUs on hello N-series VMs, see:</span></span>
    * <span data-ttu-id="09fc2-187">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (för Azure NC virtuella datorer)</span><span class="sxs-lookup"><span data-stu-id="09fc2-187">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="09fc2-188">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (för Azure NV virtuella datorer)</span><span class="sxs-lookup"><span data-stu-id="09fc2-188">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="09fc2-189">toocapture en Linux-VM-avbildning med din installerade drivrutinerna finns [hur toogeneralize och avbilda en virtuell Linux-dator](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="09fc2-189">toocapture a Linux VM image with your installed NVIDIA drivers, see [How toogeneralize and capture a Linux virtual machine](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
