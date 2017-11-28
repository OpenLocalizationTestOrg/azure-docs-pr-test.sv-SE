---
title: aaaCreate och ladda upp en OpenBSD VM bild tooAzure | Microsoft Docs
description: "Lär dig hur toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello OpenBSD operativsystemet toocreate en virtuell Azure-dator via Azure CLI"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: kyliel
ms.openlocfilehash: 0524f45ea1ecec37e55cbe9d54438a571a831de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-tooazure"></a><span data-ttu-id="29b5a-103">Skapa och ladda upp en OpenBSD disk image-tooAzure</span><span class="sxs-lookup"><span data-stu-id="29b5a-103">Create and Upload an OpenBSD disk image tooAzure</span></span>
<span data-ttu-id="29b5a-104">Den här artikeln visar hur toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello OpenBSD operativsystem.</span><span class="sxs-lookup"><span data-stu-id="29b5a-104">This article shows you how toocreate and upload a virtual hard disk (VHD) that contains hello OpenBSD operating system.</span></span> <span data-ttu-id="29b5a-105">När du har överfört kan du använda den som din egen avbildning toocreate en virtuell dator (VM) i Azure med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="29b5a-105">After you upload it, you can use it as your own image toocreate a virtual machine (VM) in Azure through Azure CLI.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="29b5a-106">Krav</span><span class="sxs-lookup"><span data-stu-id="29b5a-106">Prerequisites</span></span>
<span data-ttu-id="29b5a-107">Den här artikeln förutsätter att du har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-107">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="29b5a-108">**En Azure-prenumeration** -om du inte har ett konto kan du skapa en på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="29b5a-108">**An Azure subscription** - If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="29b5a-109">Om du har en MSDN-prenumeration, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="29b5a-109">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="29b5a-110">Annars kan du lära dig hur för[skapa ett kostnadsfritt utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="29b5a-110">Otherwise, learn how too[create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="29b5a-111">**Azure CLI 2.0** -Kontrollera att du har hello senaste [Azure CLI 2.0](/cli/azure/install-azure-cli) installerad och inloggad tooyour Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="29b5a-111">**Azure CLI 2.0** - Make sure you have hello latest [Azure CLI 2.0](/cli/azure/install-azure-cli) installed and logged in tooyour Azure account with [az login](/cli/azure/#login).</span></span>
* <span data-ttu-id="29b5a-112">**OpenBSD operativsystem i en VHD-fil** -en OpenBSD operativsystem som stöds (version 6.1) måste vara installerade tooa virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="29b5a-112">**OpenBSD operating system installed in a .vhd file** - A supported OpenBSD operating system (6.1 version) must be installed tooa virtual hard disk.</span></span> <span data-ttu-id="29b5a-113">Flera verktyg finns toocreate VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="29b5a-113">Multiple tools exist toocreate .vhd files.</span></span> <span data-ttu-id="29b5a-114">Du kan till exempel använda en virtualiseringslösning till exempel Hyper-V toocreate hello VHD-filen och installera hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="29b5a-114">For example, you can use a virtualization solution such as Hyper-V toocreate hello .vhd file and install hello operating system.</span></span> <span data-ttu-id="29b5a-115">Anvisningar om hur tooinstall och använda Hyper-V, se [installera Hyper-V och skapa en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="29b5a-115">For instructions about how tooinstall and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>


## <a name="prepare-openbsd-image-for-azure"></a><span data-ttu-id="29b5a-116">Förbereda OpenBSD bild för Azure</span><span class="sxs-lookup"><span data-stu-id="29b5a-116">Prepare OpenBSD image for Azure</span></span>
<span data-ttu-id="29b5a-117">Slutför hello följande procedurer på hello VM där du installerade hello OpenBSD operativsystemet 6.1, som läggs till Hyper-V-stöd:</span><span class="sxs-lookup"><span data-stu-id="29b5a-117">On hello VM where you installed hello OpenBSD operating system 6.1, which added Hyper-V support, complete hello following procedures:</span></span>

1. <span data-ttu-id="29b5a-118">Om DHCP inte är aktiverad under installationen, aktivera hello-tjänsten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-118">If DHCP is not enabled during installation, enable hello service as follows:</span></span>

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. <span data-ttu-id="29b5a-119">Ställ in en seriekonsolen enligt följande:</span><span class="sxs-lookup"><span data-stu-id="29b5a-119">Set up a serial console as follows:</span></span>

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. <span data-ttu-id="29b5a-120">Konfigurera installationen enligt följande:</span><span class="sxs-lookup"><span data-stu-id="29b5a-120">Configure Package installation as follows:</span></span>

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. <span data-ttu-id="29b5a-121">Som standard hello `root` användaren är inaktiverad på virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="29b5a-121">By default, hello `root` user is disabled on virtual machines in Azure.</span></span> <span data-ttu-id="29b5a-122">Användare kan köra kommandon med utökade privilegier med hjälp av hello `doas` på OpenBSD VM.</span><span class="sxs-lookup"><span data-stu-id="29b5a-122">Users can run commands with elevated privileges by using hello `doas` command on OpenBSD VM.</span></span> <span data-ttu-id="29b5a-123">Doas är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="29b5a-123">Doas is enabled by default.</span></span> <span data-ttu-id="29b5a-124">Mer information finns i [doas.conf](http://man.openbsd.org/doas.conf.5).</span><span class="sxs-lookup"><span data-stu-id="29b5a-124">For more information, see [doas.conf](http://man.openbsd.org/doas.conf.5).</span></span> 

5. <span data-ttu-id="29b5a-125">Installera och konfigurera krav för hello Azure-agenten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-125">Install and configure prerequisites for hello Azure Agent as follows:</span></span>

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. <span data-ttu-id="29b5a-126">hello senaste versionen av hello Azure-agenten alltid finns på [Github](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="29b5a-126">hello latest release of hello Azure agent can always be found on [Github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="29b5a-127">Installera hello-agenten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-127">Install hello agent as follows:</span></span>

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="29b5a-128">När du har installerat Azure-agenten är det en bra idé tooverify körs på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-128">After you install Azure Agent, it's a good idea tooverify that it's running as follows:</span></span>
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. <span data-ttu-id="29b5a-129">Avetablering hello system tooclean den och gör den lämplig för reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="29b5a-129">Deprovision hello system tooclean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="29b5a-130">hello följande kommando tas även bort hello senaste etablerade användarkontot och hello associerade data:</span><span class="sxs-lookup"><span data-stu-id="29b5a-130">hello following command also deletes hello last provisioned user account and hello associated data:</span></span>

    ```sh
    waagent -deprovision+user -force
    ```

<span data-ttu-id="29b5a-131">Nu kan du stänga av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="29b5a-131">Now you can shut down your VM.</span></span>


## <a name="prepare-hello-vhd"></a><span data-ttu-id="29b5a-132">Förbereda hello VHD</span><span class="sxs-lookup"><span data-stu-id="29b5a-132">Prepare hello VHD</span></span>
<span data-ttu-id="29b5a-133">Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="29b5a-133">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span> <span data-ttu-id="29b5a-134">Du kan konvertera hello disk toofixed VHD-format med hjälp av Hyper-V Manager eller hello Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="29b5a-134">You can convert hello disk toofixed VHD format using Hyper-V Manager or hello Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet.</span></span> <span data-ttu-id="29b5a-135">Ett exempel är följande.</span><span class="sxs-lookup"><span data-stu-id="29b5a-135">An example is as following.</span></span>

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a><span data-ttu-id="29b5a-136">Skapa storage-resurser och ladda upp</span><span class="sxs-lookup"><span data-stu-id="29b5a-136">Create storage resources and upload</span></span>
<span data-ttu-id="29b5a-137">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="29b5a-137">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="29b5a-138">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="29b5a-138">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="29b5a-139">tooupload den virtuella Hårddisken, skapa ett lagringskonto med [az storage-konto skapar](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="29b5a-139">tooupload your VHD, create a storage account with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="29b5a-140">Lagringskontonamn måste vara unikt, så ange dina egna namn.</span><span class="sxs-lookup"><span data-stu-id="29b5a-140">Storage account names must be unique, so provide your own name.</span></span> <span data-ttu-id="29b5a-141">hello följande exempel skapas ett lagringskonto med namnet *mittlagringskonto*:</span><span class="sxs-lookup"><span data-stu-id="29b5a-141">hello following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

<span data-ttu-id="29b5a-142">toocontrol åtkomst toohello lagringskontot kan du få hello lagringsnyckel med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-142">toocontrol access toohello storage account, obtain hello storage key with [az storage account keys list](/cli/azure/storage/account/keys#list) as follows:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

<span data-ttu-id="29b5a-143">toologically separat hello virtuella hårddiskar som du överför, skapa en behållare i hello lagringskonto med [az lagringsbehållaren skapa](/cli/azure/storage/container#create):</span><span class="sxs-lookup"><span data-stu-id="29b5a-143">toologically separate hello VHDs you upload, create a container within hello storage account with [az storage container create](/cli/azure/storage/container#create):</span></span>

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

<span data-ttu-id="29b5a-144">Slutligen överför den virtuella Hårddisken med [az storage blob överför](/cli/azure/storage/blob#upload) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-144">Finally, upload your VHD with [az storage blob upload](/cli/azure/storage/blob#upload) as follows:</span></span>

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a><span data-ttu-id="29b5a-145">Skapa virtuell dator från den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="29b5a-145">Create VM from your VHD</span></span>
<span data-ttu-id="29b5a-146">Du kan skapa en virtuell dator med en [exempel på skript](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) eller direkt med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="29b5a-146">You can create a VM with a [sample script](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) or directly with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="29b5a-147">toospecify hello OpenBSD VHD du överförde Använd hello `--image` parameter på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-147">toospecify hello OpenBSD VHD you uploaded, use hello `--image` parameter as follows:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="29b5a-148">Hämta hello IP-adress för din OpenBSD VM med [az vm lista-ip-adresser](/cli/azure/vm#list-ip-addresses) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-148">Obtain hello IP address for your OpenBSD VM with [az vm list-ip-addresses](/cli/azure/vm#list-ip-addresses) as follows:</span></span>

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

<span data-ttu-id="29b5a-149">Du kan nu SSH tooyour OpenBSD VM som vanligt:</span><span class="sxs-lookup"><span data-stu-id="29b5a-149">Now you can SSH tooyour OpenBSD VM as normal:</span></span>
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a><span data-ttu-id="29b5a-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29b5a-150">Next steps</span></span>
<span data-ttu-id="29b5a-151">Om du vill tooknow mer om Hyper-V stöd på OpenBSD6.1 läsa [OpenBSD 6.1](https://www.openbsd.org/61.html) och [hyperv.4](http://man.openbsd.org/hyperv.4).</span><span class="sxs-lookup"><span data-stu-id="29b5a-151">If you want tooknow more about Hyper-V support on OpenBSD6.1, read [OpenBSD 6.1](https://www.openbsd.org/61.html) and [hyperv.4](http://man.openbsd.org/hyperv.4).</span></span>

<span data-ttu-id="29b5a-152">Om du vill toocreate en virtuell dator från hanterade diskar kan läsa [az disk](/cli/azure/disk).</span><span class="sxs-lookup"><span data-stu-id="29b5a-152">If you want toocreate a VM from managed disk, read [az disk](/cli/azure/disk).</span></span> 
