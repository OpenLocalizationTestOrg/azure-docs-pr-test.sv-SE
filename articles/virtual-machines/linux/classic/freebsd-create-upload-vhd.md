---
title: aaaCreate och ladda upp en FreeBSD VM bild | Microsoft Docs
description: "Läs toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello FreeBSD operativsystemet toocreate en virtuell Azure-dator"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kyliel
ms.openlocfilehash: f3bd155e496f1a2713d36bb66ea9824ed4c210ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-freebsd-vhd-tooazure"></a><span data-ttu-id="b1431-103">Skapa och ladda upp en FreeBSD VHD-tooAzure</span><span class="sxs-lookup"><span data-stu-id="b1431-103">Create and upload a FreeBSD VHD tooAzure</span></span>
<span data-ttu-id="b1431-104">Den här artikeln visar hur toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello FreeBSD-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="b1431-104">This article shows you how toocreate and upload a virtual hard disk (VHD) that contains hello FreeBSD operating system.</span></span> <span data-ttu-id="b1431-105">När du har överfört kan du använda den som din egen avbildning toocreate en virtuell dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="b1431-105">After you upload it, you can use it as your own image toocreate a virtual machine (VM) in Azure.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="b1431-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b1431-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b1431-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b1431-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="b1431-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="b1431-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="b1431-109">Information om hur du överför en virtuell Hårddisk med hello Resource Manager-modellen finns [här](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b1431-109">For information about uploading a VHD using hello Resource Manager model, see [here](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1431-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b1431-110">Prerequisites</span></span>
<span data-ttu-id="b1431-111">Den här artikeln förutsätter att du har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b1431-111">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="b1431-112">**En Azure-prenumeration**--om du inte har ett konto kan du skapa en på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="b1431-112">**An Azure subscription**--If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="b1431-113">Om du har en MSDN-prenumeration, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="b1431-113">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="b1431-114">Annars kan du lära dig hur för[skapa ett kostnadsfritt utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1431-114">Otherwise, learn how too[create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="b1431-115">**Azure PowerShell-verktyg**--hello Azure PowerShell-modulen måste vara installerad och konfigurerad toouse din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b1431-115">**Azure PowerShell tools**--hello Azure PowerShell module must be installed and configured toouse your Azure subscription.</span></span> <span data-ttu-id="b1431-116">toodownload hello modulen, se [Azure hämtar](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b1431-116">toodownload hello module, see [Azure downloads](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="b1431-117">En genomgång som beskriver hur tooinstall och konfigurera hello modulen finns här.</span><span class="sxs-lookup"><span data-stu-id="b1431-117">A tutorial that describes how tooinstall and configure hello module is available here.</span></span> <span data-ttu-id="b1431-118">Använd hello [Azure hämtar](https://azure.microsoft.com/downloads/) cmdlet tooupload hello VHD.</span><span class="sxs-lookup"><span data-stu-id="b1431-118">Use hello [Azure Downloads](https://azure.microsoft.com/downloads/) cmdlet tooupload hello VHD.</span></span>
* <span data-ttu-id="b1431-119">**FreeBSD operativsystem i en VHD-fil**--ett operativsystem som stöds FreeBSD måste vara installerad tooa virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b1431-119">**FreeBSD operating system installed in a .vhd file**--A supported   FreeBSD operating system must be installed tooa virtual hard disk.</span></span> <span data-ttu-id="b1431-120">Flera verktyg finns toocreate VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="b1431-120">Multiple tools exist toocreate .vhd files.</span></span> <span data-ttu-id="b1431-121">Du kan till exempel använda en virtualiseringslösning till exempel Hyper-V toocreate hello VHD-filen och installera hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="b1431-121">For example, you can use a virtualization solution such as Hyper-V toocreate hello .vhd file and install hello operating system.</span></span> <span data-ttu-id="b1431-122">Anvisningar om hur tooinstall och använda Hyper-V, se [installera Hyper-V och skapa en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1431-122">For instructions about how tooinstall and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="b1431-123">hello senare VHDX-format stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="b1431-123">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="b1431-124">Du kan konvertera hello diskformat tooVHD med hjälp av Hyper-V Manager eller hello cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1431-124">You can convert hello disk tooVHD format by using Hyper-V Manager or hello cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="b1431-125">Dessutom finns en [självstudiekursen på MSDN om hur toouse FreeBSD med Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1431-125">In addition, there is a [tutorial on MSDN about how toouse FreeBSD with Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).</span></span>
>
>

<span data-ttu-id="b1431-126">Den här uppgiften innehåller hello följande fem steg:</span><span class="sxs-lookup"><span data-stu-id="b1431-126">This task includes hello following five steps:</span></span>

## <a name="step-1-prepare-hello-image-for-upload"></a><span data-ttu-id="b1431-127">Steg 1: Förbered hello avbildning för överföring</span><span class="sxs-lookup"><span data-stu-id="b1431-127">Step 1: Prepare hello image for upload</span></span>
<span data-ttu-id="b1431-128">Slutför hello följande procedurer på hello virtuell dator där du installerade hello FreeBSD-operativsystem:</span><span class="sxs-lookup"><span data-stu-id="b1431-128">On hello virtual machine where you installed hello FreeBSD operating system, complete hello following procedures:</span></span>

1. <span data-ttu-id="b1431-129">Aktivera DHCP.</span><span class="sxs-lookup"><span data-stu-id="b1431-129">Enable DHCP.</span></span>

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. <span data-ttu-id="b1431-130">Aktivera SSH.</span><span class="sxs-lookup"><span data-stu-id="b1431-130">Enable SSH.</span></span>

    <span data-ttu-id="b1431-131">Se till att hello SSH-server är installerat och konfigurerat toostart vid start.</span><span class="sxs-lookup"><span data-stu-id="b1431-131">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span> <span data-ttu-id="b1431-132">Som standard aktiveras efter installationen från FreeBSD-skiva.</span><span class="sxs-lookup"><span data-stu-id="b1431-132">By default it is enabled after installation from FreeBSD disc.</span></span> 
3. <span data-ttu-id="b1431-133">Ställ in en seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="b1431-133">Set up a serial console.</span></span>

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. <span data-ttu-id="b1431-134">Installera sudo.</span><span class="sxs-lookup"><span data-stu-id="b1431-134">Install sudo.</span></span>

    <span data-ttu-id="b1431-135">Hej rotkontot har inaktiverats i Azure.</span><span class="sxs-lookup"><span data-stu-id="b1431-135">hello root account is disabled in Azure.</span></span> <span data-ttu-id="b1431-136">Detta innebär att du behöver tooutilize sudo från en icke-privilegierade användare toorun kommandon med förhöjda privilegier.</span><span class="sxs-lookup"><span data-stu-id="b1431-136">This means you need tooutilize sudo from an unprivileged user toorun commands with elevated privileges.</span></span>

        # pkg install sudo
   
5. <span data-ttu-id="b1431-137">Förutsättningar för Azure-agenten.</span><span class="sxs-lookup"><span data-stu-id="b1431-137">Prerequisites for Azure Agent.</span></span>

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. <span data-ttu-id="b1431-138">Installera Azure-agenten.</span><span class="sxs-lookup"><span data-stu-id="b1431-138">Install Azure Agent.</span></span>

    <span data-ttu-id="b1431-139">hello senaste versionen av hello Azure-agenten alltid finns på [github](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="b1431-139">hello latest release of hello Azure Agent can always be found on [github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="b1431-140">Hej version 2.0.10 + officiellt stöder FreeBSD 10 & 10.1 och hello version 2.1.4 + (inklusive 2.2.x) officiellt stöder FreeBSD 10.2 och senare versioner.</span><span class="sxs-lookup"><span data-stu-id="b1431-140">hello version 2.0.10 + officially supports FreeBSD 10 & 10.1, and hello version 2.1.4 + (including 2.2.x) officially supports FreeBSD 10.2 and later releases.</span></span>

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    <span data-ttu-id="b1431-141">För 2.0, vi använder 2.0.16 som exempel:</span><span class="sxs-lookup"><span data-stu-id="b1431-141">For 2.0, let's use 2.0.16 as an example:</span></span>

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    <span data-ttu-id="b1431-142">För 2.1 kan vi använda 2.1.4 som exempel:</span><span class="sxs-lookup"><span data-stu-id="b1431-142">For 2.1, let's use 2.1.4 as an example:</span></span>

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > <span data-ttu-id="b1431-143">När du har installerat Azure-agenten är det en bra idé tooverify körs:</span><span class="sxs-lookup"><span data-stu-id="b1431-143">After you install Azure Agent, it's a good idea tooverify that it's running:</span></span>
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. <span data-ttu-id="b1431-144">Ta bort etableringen hello system.</span><span class="sxs-lookup"><span data-stu-id="b1431-144">Deprovision hello system.</span></span>

    <span data-ttu-id="b1431-145">Avetablering hello system tooclean den och gör den lämplig för reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="b1431-145">Deprovision hello system tooclean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="b1431-146">hello följande kommando tas även bort hello senaste etablerade användarkontot och hello associerade data:</span><span class="sxs-lookup"><span data-stu-id="b1431-146">hello following command also deletes hello last provisioned user account and hello associated data:</span></span>

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    <span data-ttu-id="b1431-147">Nu kan du stänga av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b1431-147">Now you can shut down your VM.</span></span>

## <a name="step-2-create-a-storage-account-in-azure"></a><span data-ttu-id="b1431-148">Steg 2: Skapa ett lagringskonto i Azure</span><span class="sxs-lookup"><span data-stu-id="b1431-148">Step 2: Create a storage account in Azure</span></span>
<span data-ttu-id="b1431-149">Du behöver ett lagringskonto i Azure tooupload en VHD-fil så att den kan vara används toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b1431-149">You need a storage account in Azure tooupload a .vhd file so it can be used toocreate a virtual machine.</span></span> <span data-ttu-id="b1431-150">Du kan använda hello Azure klassiska portal toocreate ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b1431-150">You can use hello Azure classic portal toocreate a storage account.</span></span>

1. <span data-ttu-id="b1431-151">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b1431-151">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="b1431-152">Markera på hello kommandofältet **ny**.</span><span class="sxs-lookup"><span data-stu-id="b1431-152">On hello command bar, select **New**.</span></span>
3. <span data-ttu-id="b1431-153">Välj **datatjänster** > **lagring** > **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="b1431-153">Select **Data Services** > **Storage** > **Quick Create**.</span></span>

    ![Snabbt skapa ett lagringskonto](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. <span data-ttu-id="b1431-155">Fyll i hello fält på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b1431-155">Fill out hello fields as follows:</span></span>

   * <span data-ttu-id="b1431-156">I hello **URL** skriver du en underdomän namn toouse i URL: en för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="b1431-156">In hello **URL** field, type a subdomain name toouse in hello storage account URL.</span></span> <span data-ttu-id="b1431-157">hello posten kan innehålla 3 till 24 siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="b1431-157">hello entry can contain from 3-24 numbers and lowercase letters.</span></span> <span data-ttu-id="b1431-158">Det här namnet blir hello värdnamn i hello-URL som används tooaddress Azure Blob storage, Azure Queue storage eller Azure Table storage-resurser för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b1431-158">This name becomes hello host name within hello URL that is used tooaddress Azure Blob storage, Azure Queue storage, or Azure Table storage resources for hello subscription.</span></span>
   * <span data-ttu-id="b1431-159">I hello **plats/Tillhörighetsgrupp** nedrullningsbara menyn, Välj hello **platsen eller tillhörighetsgruppen** för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="b1431-159">In hello **Location/Affinity Group** drop-down menu, choose hello **location or affinity group** for hello storage account.</span></span> <span data-ttu-id="b1431-160">En tillhörighetsgrupp kan du placera dina molntjänster och lagring i hello samma datacenter.</span><span class="sxs-lookup"><span data-stu-id="b1431-160">An affinity group lets you put your cloud services and storage in hello same data center.</span></span>
   * <span data-ttu-id="b1431-161">I hello **replikering** fältet, besluta om toouse **Geo-Redundant** replikering för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="b1431-161">In hello **Replication** field, decide whether toouse **Geo-Redundant** replication for hello storage account.</span></span> <span data-ttu-id="b1431-162">GEO-replikering är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="b1431-162">Geo-replication is turned on by default.</span></span> <span data-ttu-id="b1431-163">Det här alternativet replikerar data tooa sekundär plats, på tooyou utan kostnad, så att lagringen växlar toothat platsen om ett allvarligt fel inträffar sker hello primär plats.</span><span class="sxs-lookup"><span data-stu-id="b1431-163">This option replicates your data tooa secondary location, at no cost tooyou, so that your storage fails over toothat location if a major failure occurs at hello primary location.</span></span> <span data-ttu-id="b1431-164">hello sekundära platsen tilldelas automatiskt och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="b1431-164">hello secondary location is assigned automatically and can't be changed.</span></span> <span data-ttu-id="b1431-165">Om du behöver mer kontroll över hello platsen för din molnbaserade lagringsenheter på grund av toolegal krav eller organisationens principer kan inaktivera du geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="b1431-165">If you need more control over hello location of your cloud-based storage due toolegal requirements or organizational policy, you can turn off geo-replication.</span></span> <span data-ttu-id="b1431-166">Men tänk på att om du aktiverar senare geo-replikering kan du debiteras en gång dataöverföring avgift tooreplicate din befintliga data toohello sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="b1431-166">However, be aware that if you later turn on geo-replication, you will be charged a one-time data transfer fee tooreplicate your existing data toohello secondary location.</span></span> <span data-ttu-id="b1431-167">Lagringstjänster utan geo-replikering erbjuds med rabatt.</span><span class="sxs-lookup"><span data-stu-id="b1431-167">Storage services without geo-replication are offered at a discount.</span></span> <span data-ttu-id="b1431-168">Mer information om hur du hanterar geo-replikering av lagringskonton finns här: [Azure Storage-replikering](../../../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="b1431-168">More details about managing geo-replication of storage accounts can be found here: [Azure Storage replication](../../../storage/common/storage-redundancy.md).</span></span>

     ![Ange information om lagringskonto](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. <span data-ttu-id="b1431-170">Välj **skapa Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="b1431-170">Select **Create Storage Account**.</span></span> <span data-ttu-id="b1431-171">hello konto nu visas under **lagring**.</span><span class="sxs-lookup"><span data-stu-id="b1431-171">hello account now appears under **storage**.</span></span>

    ![Storage-konto har skapats](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. <span data-ttu-id="b1431-173">Skapa sedan en behållare för dina överförda VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="b1431-173">Next, create a container for your uploaded .vhd files.</span></span> <span data-ttu-id="b1431-174">Välj hello lagringskontonamn och välj sedan **behållare**.</span><span class="sxs-lookup"><span data-stu-id="b1431-174">Select hello storage account name, and then select **Containers**.</span></span>

    ![Kontoinformation för lagring](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. <span data-ttu-id="b1431-176">Välj **skapa en behållare**.</span><span class="sxs-lookup"><span data-stu-id="b1431-176">Select **Create a Container**.</span></span>

    ![Kontoinformation för lagring](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. <span data-ttu-id="b1431-178">I hello **namnet** skriver du ett namn för din behållare.</span><span class="sxs-lookup"><span data-stu-id="b1431-178">In hello **Name** field, type a name for your container.</span></span> <span data-ttu-id="b1431-179">Sedan hello **åtkomst** listrutan väljer du vilken typ av princip.</span><span class="sxs-lookup"><span data-stu-id="b1431-179">Then, in hello **Access** drop-down menu, select what type of access policy you want.</span></span>

    ![Behållarens namn](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > <span data-ttu-id="b1431-181">Som standard hello behållare är privat och kan bara användas av hello Kontoägare.</span><span class="sxs-lookup"><span data-stu-id="b1431-181">By default, hello container is private and can only be accessed by hello account owner.</span></span> <span data-ttu-id="b1431-182">tooallow offentlig läsbehörighet toohello blobbar i behållaren hello, men inte toohello behållaregenskaperna och metadata, använda hello **offentlig Blob** alternativet.</span><span class="sxs-lookup"><span data-stu-id="b1431-182">tooallow public read access toohello blobs in hello container, but not toohello container properties and metadata, use hello **Public Blob** option.</span></span> <span data-ttu-id="b1431-183">tooallow fullständig offentlig läsbehörighet för hello behållare och blobbar, använda hello **offentlig behållare** alternativet.</span><span class="sxs-lookup"><span data-stu-id="b1431-183">tooallow full public read access for hello container and blobs, use hello **Public Container** option.</span></span>
   >
   >

## <a name="step-3-prepare-hello-connection-tooazure"></a><span data-ttu-id="b1431-184">Steg 3: Förbered hello anslutning tooAzure</span><span class="sxs-lookup"><span data-stu-id="b1431-184">Step 3: Prepare hello connection tooAzure</span></span>
<span data-ttu-id="b1431-185">Innan du kan ladda upp en VHD-fil, måste tooestablish en säker anslutning mellan datorn och din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b1431-185">Before you can upload a .vhd file, you need tooestablish a secure connection between your computer and your Azure subscription.</span></span> <span data-ttu-id="b1431-186">Du kan använda metoden för hello Azure Active Directory (AD Azure) eller hello certifikat metoden toodo den.</span><span class="sxs-lookup"><span data-stu-id="b1431-186">You can use hello Azure Active Directory (Azure AD) method or hello certificate method toodo it.</span></span>

### <a name="use-hello-azure-ad-method-tooupload-a-vhd-file"></a><span data-ttu-id="b1431-187">Använd hello Azure AD-metoden tooupload en VHD-fil</span><span class="sxs-lookup"><span data-stu-id="b1431-187">Use hello Azure AD method tooupload a .vhd file</span></span>
1. <span data-ttu-id="b1431-188">Öppna hello Azure PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="b1431-188">Open hello Azure PowerShell console.</span></span>
2. <span data-ttu-id="b1431-189">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="b1431-189">Type hello following command:</span></span>  
    `Add-AzureAccount`

    <span data-ttu-id="b1431-190">Det här kommandot öppnas en inloggning där du kan logga in med ditt arbets- eller skolkonto.</span><span class="sxs-lookup"><span data-stu-id="b1431-190">This command opens a sign-in window where you can sign in with your work or school account.</span></span>

    ![PowerShell-fönster](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. <span data-ttu-id="b1431-192">Azure autentiserar och sparar hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b1431-192">Azure authenticates and saves hello credential information.</span></span> <span data-ttu-id="b1431-193">Sedan hello fönstret stängs.</span><span class="sxs-lookup"><span data-stu-id="b1431-193">Then it closes hello window.</span></span>

### <a name="use-hello-certificate-method-tooupload-a-vhd-file"></a><span data-ttu-id="b1431-194">Använd hello certifikat metoden tooupload en VHD-fil</span><span class="sxs-lookup"><span data-stu-id="b1431-194">Use hello certificate method tooupload a .vhd file</span></span>
1. <span data-ttu-id="b1431-195">Öppna hello Azure PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="b1431-195">Open hello Azure PowerShell console.</span></span>
2. <span data-ttu-id="b1431-196">Typ: `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="b1431-196">Type:  `Get-AzurePublishSettingsFile`.</span></span>
3. <span data-ttu-id="b1431-197">Ett fönster i webbläsaren öppnas och uppmanar du toodownload hello .publishsettings-fil.</span><span class="sxs-lookup"><span data-stu-id="b1431-197">A browser window opens and prompts you toodownload hello .publishsettings file.</span></span> <span data-ttu-id="b1431-198">Den här filen innehåller information och ett certifikat för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b1431-198">This file contains information and a certificate for your Azure subscription.</span></span>

    ![Hämtningssidan för webbläsare](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. <span data-ttu-id="b1431-200">Spara hello .publishsettings-fil.</span><span class="sxs-lookup"><span data-stu-id="b1431-200">Save hello .publishsettings file.</span></span>
5. <span data-ttu-id="b1431-201">Typ: `Import-AzurePublishSettingsFile <PathToFile>`, där `<PathToFile>` är hello fullständig sökväg toohello .publishsettings-fil.</span><span class="sxs-lookup"><span data-stu-id="b1431-201">Type:  `Import-AzurePublishSettingsFile <PathToFile>`, where `<PathToFile>` is hello full path toohello .publishsettings file.</span></span>

   <span data-ttu-id="b1431-202">Mer information finns i [Kom igång med Azure-cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1431-202">For more information, see [Get started with Azure cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).</span></span>

   <span data-ttu-id="b1431-203">Mer information om hur du installerar och konfigurerar PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b1431-203">For more information about installing and configuring PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="step-4-upload-hello-vhd-file"></a><span data-ttu-id="b1431-204">Steg 4: Överför hello VHD-filen</span><span class="sxs-lookup"><span data-stu-id="b1431-204">Step 4: Upload hello .vhd file</span></span>
<span data-ttu-id="b1431-205">När du överför hello VHD-filen placerar du den någonstans i Blob storage.</span><span class="sxs-lookup"><span data-stu-id="b1431-205">When you upload hello .vhd file, you can place it anywhere within your Blob storage.</span></span> <span data-ttu-id="b1431-206">Följande är vissa termer som du ska använda när du överför hello-filen:</span><span class="sxs-lookup"><span data-stu-id="b1431-206">Following are some terms you will use when you upload hello file:</span></span>

* <span data-ttu-id="b1431-207">**BlobStorageURL** är hello URL för hello storage-konto som du skapade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="b1431-207">**BlobStorageURL** is hello URL for hello storage account that you created in Step 2.</span></span>
* <span data-ttu-id="b1431-208">**YourImagesFolder** är hello behållare i Blob storage där toostore bilderna.</span><span class="sxs-lookup"><span data-stu-id="b1431-208">**YourImagesFolder** is hello container within Blob storage where you want toostore your images.</span></span>
* <span data-ttu-id="b1431-209">**VHDName** är hello etiketten som visas i hello Azure klassiska portal tooidentify hello virtuell hårddisk.</span><span class="sxs-lookup"><span data-stu-id="b1431-209">**VHDName** is hello label that appears in hello Azure classic portal tooidentify hello virtual hard disk.</span></span>
* <span data-ttu-id="b1431-210">**PathToVHDFile** hello sökvägen och namnet på hello VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="b1431-210">**PathToVHDFile** is hello full path and name of hello .vhd file.</span></span>

<span data-ttu-id="b1431-211">Du använde i föregående steg i hello hello Azure PowerShell-fönster, skriv:</span><span class="sxs-lookup"><span data-stu-id="b1431-211">From hello Azure PowerShell window you used in hello previous step, type:</span></span>

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-hello-uploaded-vhd-file"></a><span data-ttu-id="b1431-212">Steg 5: Skapa en virtuell dator med hello överförda VHD-filen</span><span class="sxs-lookup"><span data-stu-id="b1431-212">Step 5: Create a VM with hello uploaded .vhd file</span></span>
<span data-ttu-id="b1431-213">När du har överfört hello VHD-filen kan du lägga till som en bild toohello lista över anpassade avbildningar som är associerade med din prenumeration och skapa en virtuell dator med den här anpassade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="b1431-213">After you upload hello .vhd file, you can add it as an image toohello list of custom images that are associated with your subscription and create a virtual machine with this custom image.</span></span>

1. <span data-ttu-id="b1431-214">Du använde i föregående steg i hello hello Azure PowerShell-fönster, skriv:</span><span class="sxs-lookup"><span data-stu-id="b1431-214">From hello Azure PowerShell window you used in hello previous step, type:</span></span>

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of hello VHD> -OS <Type of hello OS on hello VHD>

   > [!NOTE]
   > <span data-ttu-id="b1431-215">Använda Linux som hello OS-typen.</span><span class="sxs-lookup"><span data-stu-id="b1431-215">Use Linux as hello OS type.</span></span> <span data-ttu-id="b1431-216">hello nuvarande Azure PowerShell version accepterar endast ”Linux” eller ”Windows” som en parameter.</span><span class="sxs-lookup"><span data-stu-id="b1431-216">hello current Azure PowerShell version accepts only “Linux” or “Windows” as a parameter.</span></span>
   >
   >
2. <span data-ttu-id="b1431-217">När du har slutfört föregående steg i hello hello ny avbildning visas när du väljer hello **bilder** fliken på hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b1431-217">After you complete hello previous steps, hello new image is listed when you choose hello **Images** tab on hello Azure classic portal.</span></span>  

    ![Välj en bild](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. <span data-ttu-id="b1431-219">Skapa en virtuell dator från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="b1431-219">Create a virtual machine from hello gallery.</span></span> <span data-ttu-id="b1431-220">Den här nya avbildningen är nu tillgänglig under **Mina avbildningar**.</span><span class="sxs-lookup"><span data-stu-id="b1431-220">This new image is now available under **My Images**.</span></span>
4. <span data-ttu-id="b1431-221">Välj hello ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="b1431-221">Select hello new image.</span></span> <span data-ttu-id="b1431-222">Gå sedan igenom hello prompter tooset ett värdnamn, lösenord, SSH-nyckeln och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b1431-222">Next, go through hello prompts tooset up a host name, password, SSH key, and so on.</span></span>

    ![Anpassad avbildning](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. <span data-ttu-id="b1431-224">När du har slutfört hello etablering ser du ditt FreeBSD VM körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="b1431-224">After you complete hello provisioning, you'll see your FreeBSD VM running in Azure.</span></span>

    ![FreeBSD-avbildning i azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
