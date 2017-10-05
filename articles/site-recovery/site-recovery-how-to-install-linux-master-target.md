---
title: "Så här installerar du en Linux-huvudmålserver för redundans från Azure till lokala | Microsoft Docs"
description: "Innan du skydda en Linux-dator, måste en Linux-huvudmålserver. Lär dig hur du installerar en."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 5341e3e56e0c366079958dd9a885f6ee3e8436cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="install-a-linux-master-target-server"></a><span data-ttu-id="a45a1-104">Installera en Linux-huvudmålsserver</span><span class="sxs-lookup"><span data-stu-id="a45a1-104">Install a Linux master target server</span></span>
<span data-ttu-id="a45a1-105">När du växlar över dina virtuella datorer kan du växla tillbaka de virtuella datorerna till den lokala platsen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-105">After you fail over your virtual machines, you can fail back the virtual machines to the on-premises site.</span></span> <span data-ttu-id="a45a1-106">För att växla tillbaka måste att skydda den virtuella datorn från Azure till den lokala platsen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-106">To fail back, you need to reprotect the virtual machine from Azure to the on-premises site.</span></span> <span data-ttu-id="a45a1-107">För den här processen behöver du en lokal huvudmålservern för att ta emot trafiken.</span><span class="sxs-lookup"><span data-stu-id="a45a1-107">For this process, you need an on-premises master target server to receive the traffic.</span></span> 

<span data-ttu-id="a45a1-108">Om den skydda virtuella datorn är en Windows-dator, måste en Windows-huvudmålserver.</span><span class="sxs-lookup"><span data-stu-id="a45a1-108">If your protected virtual machine is a Windows virtual machine, then you need a Windows master target.</span></span> <span data-ttu-id="a45a1-109">För en virtuell Linux-dator behöver du en Linux-huvudmålserverns.</span><span class="sxs-lookup"><span data-stu-id="a45a1-109">For a Linux virtual machine, you need a Linux master target.</span></span> <span data-ttu-id="a45a1-110">Läs följande information om hur du skapar och installerar en Linux-huvudmålserverns.</span><span class="sxs-lookup"><span data-stu-id="a45a1-110">Read the following steps to learn how to create and install a Linux master target.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a45a1-111">Från och med lanseringen av 9.10.0 huvudmålservern, senaste huvudmålservern kan endast installeras på en 16.04 Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="a45a1-111">Starting with release of the 9.10.0 master target server, the latest master target server can be only installed on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="a45a1-112">Nya installationer är inte tillåtna på CentOS6.6 servrar.</span><span class="sxs-lookup"><span data-stu-id="a45a1-112">New installations aren't allowed on  CentOS6.6 servers.</span></span> <span data-ttu-id="a45a1-113">Du kan dock fortsätta att uppgradera din gamla huvudnyckeln målservrar med hjälp av 9.10.0 version.</span><span class="sxs-lookup"><span data-stu-id="a45a1-113">However, you can continue to upgrade your old master target servers by using the 9.10.0 version.</span></span>

## <a name="overview"></a><span data-ttu-id="a45a1-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="a45a1-114">Overview</span></span>
<span data-ttu-id="a45a1-115">Den här artikeln innehåller anvisningar för hur du installerar en Linux-huvudmålserverns.</span><span class="sxs-lookup"><span data-stu-id="a45a1-115">This article provides instructions for how to install a Linux master target.</span></span>

<span data-ttu-id="a45a1-116">Skicka kommentarer eller frågor i slutet av den här artikeln eller på den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="a45a1-116">Post comments or questions at the end of this article or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a45a1-117">Krav</span><span class="sxs-lookup"><span data-stu-id="a45a1-117">Prerequisites</span></span>

* <span data-ttu-id="a45a1-118">Avgör om återställningen kommer att vara till en befintlig lokal virtuell dator eller till en ny virtuell dator för att välja den värd som du vill distribuera huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-118">To choose the host on which to deploy the master target, determine if the failback is going to be to an existing on-premises virtual machine or to a new virtual machine.</span></span> 
    * <span data-ttu-id="a45a1-119">För en befintlig virtuell dator ska värden på Huvudmålet ha åtkomst till datalager på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a45a1-119">For an existing virtual machine, the host of the master target should have access to the data stores of the virtual machine.</span></span>
    * <span data-ttu-id="a45a1-120">Om den lokala virtuella datorn inte finns, skapas den virtuella datorn för återställning efter fel på samma värddator som huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-120">If the on-premises virtual machine does not exist, the failback virtual machine is created on the same host as the master target.</span></span> <span data-ttu-id="a45a1-121">Du kan välja alla ESXi-värd för att installera huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-121">You can choose any ESXi host to install the master target.</span></span>
* <span data-ttu-id="a45a1-122">Huvudmålservern ska vara i ett nätverk som kan kommunicera med processervern och konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-122">The master target should be on a network that can communicate with the process server and the configuration server.</span></span>
* <span data-ttu-id="a45a1-123">Versionen av huvudmålservern måste vara lika med eller tidigare versioner av processervern och konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-123">The version of the master target must be equal to or earlier than the versions of the process server and the configuration server.</span></span> <span data-ttu-id="a45a1-124">Till exempel om versionen av konfigurationsservern är 9.4, kan versionen av huvudmålservern vara 9.4 eller 9.3 men inte 9.5.</span><span class="sxs-lookup"><span data-stu-id="a45a1-124">For example, if the version of the configuration server is 9.4, the version of the master target can be 9.4 or 9.3 but not 9.5.</span></span>
* <span data-ttu-id="a45a1-125">Huvudmålservern kan bara finnas en virtuell VMware-dator och inte en fysisk server.</span><span class="sxs-lookup"><span data-stu-id="a45a1-125">The master target can only be a VMware virtual machine and not a physical server.</span></span>

## <a name="create-the-master-target-according-to-the-sizing-guidelines"></a><span data-ttu-id="a45a1-126">Skapa huvudmålservern enligt riktlinjerna som storlek</span><span class="sxs-lookup"><span data-stu-id="a45a1-126">Create the master target according to the sizing guidelines</span></span>

<span data-ttu-id="a45a1-127">Skapa huvudmålservern i enlighet med riktlinjerna nedan storlek:</span><span class="sxs-lookup"><span data-stu-id="a45a1-127">Create the master target in accordance with the following sizing guidelines:</span></span>
- <span data-ttu-id="a45a1-128">**RAM-minne**: 6 GB eller mer</span><span class="sxs-lookup"><span data-stu-id="a45a1-128">**RAM**: 6 GB or more</span></span>
- <span data-ttu-id="a45a1-129">**OS-diskstorlek**: 100 GB eller mer (för att installera CentOS6.6)</span><span class="sxs-lookup"><span data-stu-id="a45a1-129">**OS disk size**: 100 GB or more (to install CentOS6.6)</span></span>
- <span data-ttu-id="a45a1-130">**Ytterligare diskstorleken för kvarhållningsenhetens**: 1 TB</span><span class="sxs-lookup"><span data-stu-id="a45a1-130">**Additional disk size for retention drive**: 1 TB</span></span>
- <span data-ttu-id="a45a1-131">**CPU-kärnor**: 4 kärnor eller mer</span><span class="sxs-lookup"><span data-stu-id="a45a1-131">**CPU cores**: 4 cores or more</span></span>

<span data-ttu-id="a45a1-132">Följande stöds Ubuntu kärnor stöds.</span><span class="sxs-lookup"><span data-stu-id="a45a1-132">The following supported Ubuntu kernels are supported.</span></span>


|<span data-ttu-id="a45a1-133">Kernel-serien</span><span class="sxs-lookup"><span data-stu-id="a45a1-133">Kernel Series</span></span>  |<span data-ttu-id="a45a1-134">Stöd för upp till</span><span class="sxs-lookup"><span data-stu-id="a45a1-134">Support up to</span></span>  |
|---------|---------|
|<span data-ttu-id="a45a1-135">4.4</span><span class="sxs-lookup"><span data-stu-id="a45a1-135">4.4</span></span>      |<span data-ttu-id="a45a1-136">4.4.0-81-Generic</span><span class="sxs-lookup"><span data-stu-id="a45a1-136">4.4.0-81-generic</span></span>         |
|<span data-ttu-id="a45a1-137">4.8</span><span class="sxs-lookup"><span data-stu-id="a45a1-137">4.8</span></span>      |<span data-ttu-id="a45a1-138">4.8.0-56-Generic</span><span class="sxs-lookup"><span data-stu-id="a45a1-138">4.8.0-56-generic</span></span>         |
|<span data-ttu-id="a45a1-139">4.10</span><span class="sxs-lookup"><span data-stu-id="a45a1-139">4.10</span></span>     |<span data-ttu-id="a45a1-140">4.10.0-24-Generic</span><span class="sxs-lookup"><span data-stu-id="a45a1-140">4.10.0-24-generic</span></span>        |


## <a name="deploy-the-master-target-server"></a><span data-ttu-id="a45a1-141">Distribuera huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a45a1-141">Deploy the master target server</span></span>

### <a name="install-ubuntu-16042-minimal"></a><span data-ttu-id="a45a1-142">Installera Ubuntu 16.04.2 Minimal</span><span class="sxs-lookup"><span data-stu-id="a45a1-142">Install Ubuntu 16.04.2 Minimal</span></span>

<span data-ttu-id="a45a1-143">Vidta följande steg för att installera Ubuntu 16.04.2 64-bitars operativsystem.</span><span class="sxs-lookup"><span data-stu-id="a45a1-143">Take the following the steps to install the Ubuntu 16.04.2 64-bit operating system.</span></span>

<span data-ttu-id="a45a1-144">**Steg 1:** går du till den [Hämta länk](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) och välj den närmaste spegling från vilka hämta en Ubuntu 16.04.2 minimal 64-bitars ISO.</span><span class="sxs-lookup"><span data-stu-id="a45a1-144">**Step 1:** Go to the [download link](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) and choose the closest mirror from which download an Ubuntu 16.04.2 minimal 64-bit ISO.</span></span>

<span data-ttu-id="a45a1-145">Behåll en Ubuntu 16.04.2 minimal 64-bitars ISO i DVD-enheten och starta systemet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-145">Keep an Ubuntu 16.04.2 minimal 64-bit ISO in the DVD drive and start the system.</span></span>

<span data-ttu-id="a45a1-146">**Steg 2:** Välj **engelska** som önskat språk och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-146">**Step 2:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Välj ett språk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

<span data-ttu-id="a45a1-148">**Steg 3:** Välj **installerar Ubuntu Server**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-148">**Step 3:** Select **Install Ubuntu Server**, and then select **Enter**.</span></span>

![Välj installerar Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

<span data-ttu-id="a45a1-150">**Steg 4:** Välj **engelska** som önskat språk och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-150">**Step 4:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Välj engelska som ditt språk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

<span data-ttu-id="a45a1-152">**Steg 5:** Välj lämpligt alternativ från den **tidszon** alternativ, och välj **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-152">**Step 5:** Select the appropriate option from the **Time Zone** options list, and then select **Enter**.</span></span>

![Välj rätt tidszon](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

<span data-ttu-id="a45a1-154">**Steg 6:** Välj **nr** (standardalternativet) och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-154">**Step 6:** Select **No** (the default option), and then select **Enter**.</span></span>


![Konfigurera tangentbordet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

<span data-ttu-id="a45a1-156">**Steg 7:** Välj **engelska (USA)** som land för tangentbord och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-156">**Step 7:** Select **English (US)** as the country of origin for the keyboard, and then select **Enter**.</span></span>

![Välj USA som land för ursprung](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

<span data-ttu-id="a45a1-158">**Steg 8:** Välj **engelska (USA)** som tangentbordslayout och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-158">**Step 8:** Select **English (US)** as the keyboard layout, and then select **Enter**.</span></span>

![Välj amerikansk engelska som tangentbordslayout](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

<span data-ttu-id="a45a1-160">**Steg 9:** ange värdnamnet för servern i den **värdnamn** och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-160">**Step 9:** Enter the hostname for your server in the **Hostname** box, and then select **Continue**.</span></span>

![Ange värdnamnet för servern](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

<span data-ttu-id="a45a1-162">**Steg 10:** ange användarnamnet om du vill skapa ett användarkonto, och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-162">**Step 10:** To create a user account, enter the user name, and then select **Continue**.</span></span>

![Skapa ett användarkonto](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

<span data-ttu-id="a45a1-164">**Steg 11:** ange lösenordet för det nya användarkontot och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-164">**Step 11:** Enter the password for the new user account, and then select **Continue**.</span></span>

![Ange lösenordet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

<span data-ttu-id="a45a1-166">**Steg 12:** bekräftar du lösenordet för den nya användaren och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-166">**Step 12:** Confirm the password for the new user, and then select **Continue**.</span></span>

![Bekräfta lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

<span data-ttu-id="a45a1-168">**Steg 13:** Välj **nr** (standardalternativet) och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-168">**Step 13:** Select **No** (the default option), and then select **Enter**.</span></span>

![Konfigurera användare och lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

<span data-ttu-id="a45a1-170">**Steg 14:** om tidszonen som visas är korrekt, Välj **Ja** (standardalternativet) och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-170">**Step 14:** If the time zone that's displayed is correct, select **Yes** (the default option), and then select **Enter**.</span></span>

<span data-ttu-id="a45a1-171">Om du vill konfigurera om tidszonen, Välj **nr**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-171">To reconfigure your time zone, select **No**.</span></span>

![Konfigurera klockan](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

<span data-ttu-id="a45a1-173">**Steg 15:** alternativen partitionering metoden Välj **interaktiv - använder hela disken**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-173">**Step 15:** From the partitioning method options, select **Guided - use entire disk**, and then select **Enter**.</span></span>

![Välj metodalternativet partitionering](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

<span data-ttu-id="a45a1-175">**Steg 16:** välja en lämplig disk från den **väljer disk till partition** alternativ och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-175">**Step 16:** Select the appropriate disk from the **Select disk to partition** options, and then select **Enter**.</span></span>


![Välj disken](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

<span data-ttu-id="a45a1-177">**Steg 17:** Välj **Ja** att skriva ändringar till disk och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-177">**Step 17:** Select **Yes** to write the changes to disk, and then select **Enter**.</span></span>

![För att skriva ändringar till disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

<span data-ttu-id="a45a1-179">**Steg 18:** väljer standardalternativet, väljer **Fortsätt**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-179">**Step 18:** Select the default option, select **Continue**, and then select **Enter**.</span></span>

![Välj alternativet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

<span data-ttu-id="a45a1-181">**Steg 19:** Välj lämpligt alternativ för att hantera uppgraderingar på datorn och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-181">**Step 19:** Select the appropriate option for managing upgrades on your system, and then select **Enter**.</span></span>

![Välj hur du hanterar uppgraderingar](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> <span data-ttu-id="a45a1-183">Eftersom Azure Site Recovery huvudmålservern kräver en särskild version av Ubuntu, måste du se till att kernel uppgraderingar har inaktiverats för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a45a1-183">Because the Azure Site Recovery master target server requires a very specific version of the Ubuntu, you need to ensure that the kernel upgrades are disabled for the virtual machine.</span></span> <span data-ttu-id="a45a1-184">Om de är aktiverade kan orsaka reguljära uppgraderingar huvudmålservern inte fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="a45a1-184">If they are enabled, then any regular upgrades cause the master target server to malfunction.</span></span> <span data-ttu-id="a45a1-185">Kontrollera att du väljer den **inga automatiska uppdateringar** alternativet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-185">Make sure you select the **No automatic updates** option.</span></span>


<span data-ttu-id="a45a1-186">**Steg 20:** Välj standardalternativen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-186">**Step 20:** Select default options.</span></span> <span data-ttu-id="a45a1-187">Om du vill openSSH för SSH ansluta, Välj den **OpenSSH server** alternativet och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-187">If you want openSSH for SSH connect, select the **OpenSSH server** option, and then select **Continue**.</span></span>

![Välj program](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

<span data-ttu-id="a45a1-189">**Steg 21:** Välj **Ja**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-189">**Step 21:** Select **Yes**, and then select **Enter**.</span></span>

![Installationen GRUB startprogram](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

<span data-ttu-id="a45a1-191">**Steg 22:** väljer enheten för start inläsaren installation (helst **/dev/sda**), och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-191">**Step 22:** Select the appropriate device for the boot loader installation (preferably **/dev/sda**), and then select **Enter**.</span></span>

![Välj en enhet för start inläsaren installation](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

<span data-ttu-id="a45a1-193">**Steg 23:** Välj **Fortsätt**, och välj sedan **RETUR** att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-193">**Step 23:** Select **Continue**, and then select **Enter** to finish the installation.</span></span>

![Slut installationen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

<span data-ttu-id="a45a1-195">När installationen är klar kan du logga in på den virtuella datorn med de nya användarautentiseringsuppgifterna för.</span><span class="sxs-lookup"><span data-stu-id="a45a1-195">After the installation has finished, sign in to the VM with the new user credentials.</span></span> <span data-ttu-id="a45a1-196">(Se **steg 10** för mer information.)</span><span class="sxs-lookup"><span data-stu-id="a45a1-196">(Refer to **Step 10** for more information.)</span></span>

<span data-ttu-id="a45a1-197">Vidta de åtgärder som beskrivs i följande skärmbild att ange rotlösenordet för användaren.</span><span class="sxs-lookup"><span data-stu-id="a45a1-197">Take the steps that are described in the following screenshot to set the ROOT user password.</span></span> <span data-ttu-id="a45a1-198">Logga in som rotanvändare.</span><span class="sxs-lookup"><span data-stu-id="a45a1-198">Then sign in as ROOT user.</span></span>

![Ange ROTEN användarlösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-the-machine-for-configuration-as-a-master-target-server"></a><span data-ttu-id="a45a1-200">Förbered datorn för konfigurationen som en huvudmålserver</span><span class="sxs-lookup"><span data-stu-id="a45a1-200">Prepare the machine for configuration as a master target server</span></span>
<span data-ttu-id="a45a1-201">Förbered datorn för konfigurationen som en huvudmålserver.</span><span class="sxs-lookup"><span data-stu-id="a45a1-201">Next, prepare the machine for configuration as a master target server.</span></span>

<span data-ttu-id="a45a1-202">Om du vill hämta ID för varje SCSI-hårddisk på en virtuell Linux-dator, aktivera den **disk. EnableUUID = TRUE** parameter.</span><span class="sxs-lookup"><span data-stu-id="a45a1-202">To get the ID for each SCSI hard disk in a Linux virtual machine, enable the **disk.EnableUUID = TRUE** parameter.</span></span>

<span data-ttu-id="a45a1-203">Om du vill aktivera den här parametern, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="a45a1-203">To enable this parameter, take the following steps:</span></span>

1. <span data-ttu-id="a45a1-204">Stäng av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a45a1-204">Shut down your virtual machine.</span></span>

2. <span data-ttu-id="a45a1-205">Högerklicka på posten för den virtuella datorn i den vänstra rutan och välj sedan **redigera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-205">Right-click the entry for the virtual machine in the left pane, and then select **Edit Settings**.</span></span>

3. <span data-ttu-id="a45a1-206">Välj den **alternativ** fliken.</span><span class="sxs-lookup"><span data-stu-id="a45a1-206">Select the **Options** tab.</span></span>

4. <span data-ttu-id="a45a1-207">I den vänstra rutan, Välj **Avancerat** > **allmänna**, och välj sedan den **konfigurationsparametrar** -knappen i den nedre högra delen av skärmen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-207">In the left pane, select **Advanced** > **General**, and then select the **Configuration Parameters** button on the lower-right part of the screen.</span></span>

    ![Alternativfliken](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    <span data-ttu-id="a45a1-209">Den **konfigurationsparametrar** alternativet är inte tillgängligt när datorn körs.</span><span class="sxs-lookup"><span data-stu-id="a45a1-209">The **Configuration Parameters** option is not available when the machine is running.</span></span> <span data-ttu-id="a45a1-210">Stäng av den virtuella datorn om du vill aktivera den här fliken.</span><span class="sxs-lookup"><span data-stu-id="a45a1-210">To make this tab active, shut down the virtual machine.</span></span>

5. <span data-ttu-id="a45a1-211">Se om en rad med **disk. EnableUUID** finns redan.</span><span class="sxs-lookup"><span data-stu-id="a45a1-211">See whether a row with **disk.EnableUUID** already exists.</span></span>

    - <span data-ttu-id="a45a1-212">Om värdet finns och är inställd på **FALSKT**, ändra värdet till **SANT**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-212">If the value exists and is set to **False**, change the value to **True**.</span></span> <span data-ttu-id="a45a1-213">(Värdena är inte skiftlägeskänsligt.)</span><span class="sxs-lookup"><span data-stu-id="a45a1-213">(The values are not case-sensitive.)</span></span>

    - <span data-ttu-id="a45a1-214">Om värdet finns och är inställd på **SANT**väljer **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-214">If the value exists and is set to **True**, select **Cancel**.</span></span>

    - <span data-ttu-id="a45a1-215">Om värdet inte finns, väljer **Lägg till rad**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-215">If the value does not exist, select **Add Row**.</span></span>

    - <span data-ttu-id="a45a1-216">Lägg till i namnkolumnen **disk. EnableUUID**, och ange värdet **SANT**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-216">In the name column, add **disk.EnableUUID**, and then set the value to **TRUE**.</span></span>

    ![Kontrollerar om disken. EnableUUID finns redan](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a><span data-ttu-id="a45a1-218">Inaktivera kernel-uppgraderingar</span><span class="sxs-lookup"><span data-stu-id="a45a1-218">Disable kernel upgrades</span></span>

<span data-ttu-id="a45a1-219">Azure Site Recovery-huvudmålservern kräver en särskild version av Ubuntu, se till att kernel-uppgraderingar är inaktiverat för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a45a1-219">Azure Site Recovery master target server requires a very specific version of the Ubuntu, ensure that the kernel upgrades are disabled for the virtual machine.</span></span>

<span data-ttu-id="a45a1-220">Om kernel uppgraderingar är aktiverade kan orsaka reguljära uppgraderingar huvudmålservern inte fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="a45a1-220">If kernel upgrades are enabled, then any regular upgrades cause the master target server to malfunction.</span></span>

#### <a name="download-and-install-additional-packages"></a><span data-ttu-id="a45a1-221">Hämta och installera ytterligare paket</span><span class="sxs-lookup"><span data-stu-id="a45a1-221">Download and install additional packages</span></span>

> [!NOTE]
> <span data-ttu-id="a45a1-222">Kontrollera att du har Internetanslutning att hämta och installera ytterligare paket.</span><span class="sxs-lookup"><span data-stu-id="a45a1-222">Make sure that you have Internet connectivity to download and install additional packages.</span></span> <span data-ttu-id="a45a1-223">Om du inte har Internetanslutning, måste du söka efter dessa RPM-paket och installera dem manuellt.</span><span class="sxs-lookup"><span data-stu-id="a45a1-223">If you don't have Internet connectivity, you need to manually find these RPM packages and install them.</span></span>

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-the-installer-for-setup"></a><span data-ttu-id="a45a1-224">Hämta installationsprogrammet för installation</span><span class="sxs-lookup"><span data-stu-id="a45a1-224">Get the installer for setup</span></span>

<span data-ttu-id="a45a1-225">Om din huvudmålservern har Internetanslutning, kan du använda följande steg för att hämta installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-225">If your master target has Internet connectivity, you can use the following steps to download the installer.</span></span> <span data-ttu-id="a45a1-226">Annars kan du kopiera installationsprogrammet från processervern och installera den.</span><span class="sxs-lookup"><span data-stu-id="a45a1-226">Otherwise, you can copy the installer from the process server and then install it.</span></span>

#### <a name="download-the-master-target-installation-packages"></a><span data-ttu-id="a45a1-227">Hämta huvudmålservern-paket</span><span class="sxs-lookup"><span data-stu-id="a45a1-227">Download the master target installation packages</span></span>

<span data-ttu-id="a45a1-228">[Hämta den senaste Linux huvudmålserver installation bits](https://aka.ms/latestlinuxmobsvc).</span><span class="sxs-lookup"><span data-stu-id="a45a1-228">[Download the latest Linux master target installation bits](https://aka.ms/latestlinuxmobsvc).</span></span>

<span data-ttu-id="a45a1-229">Om du vill hämta den med hjälp av Linux, skriver du:</span><span class="sxs-lookup"><span data-stu-id="a45a1-229">To download it by using Linux, type:</span></span>

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

<span data-ttu-id="a45a1-230">Kontrollera att du hämtar och packa installationsprogrammet i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-230">Make sure that you download and unzip the installer in your home directory.</span></span> <span data-ttu-id="a45a1-231">Om du packa upp till **/usr/lokal**, misslyckas installationen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-231">If you unzip to **/usr/Local**, then the installation  fails.</span></span>


#### <a name="access-the-installer-from-the-process-server"></a><span data-ttu-id="a45a1-232">Åtkomst till installationsprogrammet från processervern</span><span class="sxs-lookup"><span data-stu-id="a45a1-232">Access the installer from the process server</span></span>

1. <span data-ttu-id="a45a1-233">På processervern, gå till **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-233">On the process server, go to **C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span></span>

2. <span data-ttu-id="a45a1-234">Kopiera den nödvändiga installer-fil från processervern och spara den som **latestlinuxmobsvc.tar.gz** i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-234">Copy the required installer file from the process server, and save it as **latestlinuxmobsvc.tar.gz** in your home directory.</span></span>


### <a name="apply-custom-configuration-changes"></a><span data-ttu-id="a45a1-235">Använd anpassad konfigurationsändringar</span><span class="sxs-lookup"><span data-stu-id="a45a1-235">Apply custom configuration changes</span></span>

<span data-ttu-id="a45a1-236">Om du vill använda anpassade konfigurationsändringar, använder du följande steg:</span><span class="sxs-lookup"><span data-stu-id="a45a1-236">To apply custom configuration changes, use the following steps:</span></span>


1. <span data-ttu-id="a45a1-237">Kör följande kommando för att untar binärfilen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-237">Run the following command to untar the binary.</span></span>
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Skärmbild av kommandot ska köras](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. <span data-ttu-id="a45a1-239">Kör följande kommando för att ge behörighet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-239">Run the following command to give permission.</span></span>
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. <span data-ttu-id="a45a1-240">Kör följande kommando för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-240">Run the following command to run the script.</span></span>
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> <span data-ttu-id="a45a1-241">Kör skriptet en gång på servern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-241">Run the script only once on the server.</span></span> <span data-ttu-id="a45a1-242">Stänga av servern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-242">Shut down the server.</span></span> <span data-ttu-id="a45a1-243">Starta om servern när du lägger till en disk, enligt beskrivningen i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a45a1-243">Then restart the server after you add a disk, as described in the next section.</span></span>

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a><span data-ttu-id="a45a1-244">Lägg till en disk för kvarhållning Linux huvudmålservern virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a45a1-244">Add a retention disk to the Linux master target virtual machine</span></span>

<span data-ttu-id="a45a1-245">Använd följande steg för att skapa en kvarhållningsdisken:</span><span class="sxs-lookup"><span data-stu-id="a45a1-245">Use the following steps to create a retention disk:</span></span>

1. <span data-ttu-id="a45a1-246">Koppla en ny disk 1 TB till Linux huvudmålservern virtuell dator och sedan starta datorn.</span><span class="sxs-lookup"><span data-stu-id="a45a1-246">Attach a new 1-TB disk to the Linux master target virtual machine, and then start the machine.</span></span>

2. <span data-ttu-id="a45a1-247">Använd den **multipath -lla** kommandot Läs multipath ID för kvarhållningsdisken.</span><span class="sxs-lookup"><span data-stu-id="a45a1-247">Use the **multipath -ll** command to learn the multipath ID of the retention disk.</span></span>

    ```
    multipath -ll
    ```
    ![Flera sökvägar kvarhållningsdisken-ID](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. <span data-ttu-id="a45a1-249">Formatera hårddisken och sedan skapa ett filsystem i den nya enheten.</span><span class="sxs-lookup"><span data-stu-id="a45a1-249">Format the drive, and then create a file system on the new drive.</span></span>

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Skapa ett filsystem på enheten](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. <span data-ttu-id="a45a1-251">När du har skapat filsystemet montera kvarhållningsdisken.</span><span class="sxs-lookup"><span data-stu-id="a45a1-251">After you create the file system, mount the retention disk.</span></span>
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Montera kvarhållningsdisken](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. <span data-ttu-id="a45a1-253">Skapa den **fstab** post att montera kvarhållningsenhetens varje gång systemet startar.</span><span class="sxs-lookup"><span data-stu-id="a45a1-253">Create the **fstab** entry to mount the retention drive every time the system starts.</span></span>
    ```
    vi /etc/fstab
    ```
    <span data-ttu-id="a45a1-254">Välj **infoga** att börja redigera filen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-254">Select **Insert** to begin editing the file.</span></span> <span data-ttu-id="a45a1-255">Skapa en ny rad och Lägg till följande text.</span><span class="sxs-lookup"><span data-stu-id="a45a1-255">Create a new line, and then insert the following text.</span></span> <span data-ttu-id="a45a1-256">Redigera disk multipath-ID baserat på den markerade multipath ID från det föregående kommandot.</span><span class="sxs-lookup"><span data-stu-id="a45a1-256">Edit the disk multipath ID based on the highlighted multipath ID from the previous command.</span></span>

    <span data-ttu-id="a45a1-257"> **/dev/mapper/ <Retention disks multipath id> /mnt/kvarhållning ext4 rw 0 0**</span><span class="sxs-lookup"><span data-stu-id="a45a1-257">**/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**</span></span>

    <span data-ttu-id="a45a1-258">Välj **Esc**, och skriv sedan **: wq** (skriva och avsluta) att stänga fönstret redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a45a1-258">Select **Esc**, and then type **:wq** (write and quit) to close the editor window.</span></span>

### <a name="install-the-master-target"></a><span data-ttu-id="a45a1-259">Installera huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a45a1-259">Install the master target</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a45a1-260">Versionen av huvudmålservern måste vara lika med eller tidigare versioner av processervern och konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-260">The version of the master target server must be equal to or earlier than the versions of the process server and the configuration server.</span></span> <span data-ttu-id="a45a1-261">Om detta inte är uppfyllt, skyddar lyckas, men kan slutföras inte.</span><span class="sxs-lookup"><span data-stu-id="a45a1-261">If this condition is not met, reprotect succeeds, but replication fails.</span></span>


> [!NOTE]
> <span data-ttu-id="a45a1-262">Innan du installerar huvudmålservern måste du kontrollera att den **/etc/hosts** filen på den virtuella datorn innehåller poster som motsvarar IP-adresser som är associerade med alla nätverkskort för det lokala värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-262">Before you install the master target server, check that the **/etc/hosts** file on the virtual machine contains entries that map the local hostname to the IP addresses that are associated with all network adapters.</span></span>

1. <span data-ttu-id="a45a1-263">Kopiera lösenfras från **C:\ProgramData\Microsoft Azure plats Recovery\private\connection.passphrase** på konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-263">Copy the passphrase from **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** on the configuration server.</span></span> <span data-ttu-id="a45a1-264">Spara den som **passphrase.txt** i samma lokala katalog genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a45a1-264">Then save it as **passphrase.txt** in the same local directory by running the following command:</span></span>

    ```
    echo <passphrase> >passphrase.txt
    ```
    <span data-ttu-id="a45a1-265">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a45a1-265">Example:</span></span> 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. <span data-ttu-id="a45a1-266">Observera configuration-serverns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a45a1-266">Note the configuration server's IP address.</span></span> <span data-ttu-id="a45a1-267">Du behöver den i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a45a1-267">You need it in the next step.</span></span>

3. <span data-ttu-id="a45a1-268">Kör följande kommando för att installera huvudmålservern och registrera servern med konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-268">Run the following command to install the master target server and register the server with the configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    <span data-ttu-id="a45a1-269">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a45a1-269">Example:</span></span> 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    <span data-ttu-id="a45a1-270">Vänta tills skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a45a1-270">Wait until the script finishes.</span></span> <span data-ttu-id="a45a1-271">Om huvudmålservern registrerar konfigurationsändringarna, huvudmålservern finns på den **Site Recovery-infrastruktur** i portalen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-271">If the master target registers sucessfully, the master target is listed on the **Site Recovery Infrastructure** page of the portal.</span></span>


#### <a name="install-the-master-target-by-using-interactive-installation"></a><span data-ttu-id="a45a1-272">Installera huvudmålservern genom att använda interaktiv installation</span><span class="sxs-lookup"><span data-stu-id="a45a1-272">Install the master target by using interactive installation</span></span>

1. <span data-ttu-id="a45a1-273">Kör följande kommando för att installera huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-273">Run the following command to install the master target.</span></span> <span data-ttu-id="a45a1-274">Rollen agent väljer **Huvudmålservern**.</span><span class="sxs-lookup"><span data-stu-id="a45a1-274">For the agent role, choose **Master Target**.</span></span>

    ```
    ./install
    ```

2. <span data-ttu-id="a45a1-275">Välj standardplatsen för installation och välj sedan **RETUR** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="a45a1-275">Choose the default location for installation, and then select **Enter** to continue.</span></span>

    ![Om du väljer en standardplatsen för installation av huvudmålservern](./media/site-recovery-how-to-install-linux-master-target/image17.png)

<span data-ttu-id="a45a1-277">När installationen är klar kan du registrera konfigurationsservern med hjälp av kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="a45a1-277">After the installation has finished, register the configuration server by using the command line.</span></span>

1. <span data-ttu-id="a45a1-278">Observera IP-adress för konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-278">Note the IP address of the configuration server.</span></span> <span data-ttu-id="a45a1-279">Du behöver den i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a45a1-279">You need it in the next step.</span></span>

2. <span data-ttu-id="a45a1-280">Kör följande kommando för att installera huvudmålservern och registrera servern med konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-280">Run the following command to install the master target server and register the server with the configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    <span data-ttu-id="a45a1-281">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a45a1-281">Example:</span></span> 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   <span data-ttu-id="a45a1-282">Vänta tills skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a45a1-282">Wait until the script finishes.</span></span> <span data-ttu-id="a45a1-283">Om huvudmålservern är registrerade har, huvudmålservern finns på den **Site Recovery-infrastruktur** i portalen.</span><span class="sxs-lookup"><span data-stu-id="a45a1-283">If the master target is registered succesfully, the master target is listed on the **Site Recovery Infrastructure** page of the portal.</span></span>


### <a name="upgrade-the-master-target"></a><span data-ttu-id="a45a1-284">Uppgradera huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a45a1-284">Upgrade the master target</span></span>

<span data-ttu-id="a45a1-285">Kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-285">Run the installer.</span></span> <span data-ttu-id="a45a1-286">Den upptäcker automatiskt att agenten är installerad på huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-286">It automatically detects that the agent is installed on the master target.</span></span> <span data-ttu-id="a45a1-287">Om du vill uppgradera, Välj **Y**.  När installationen har slutförts, kontrollerar du vilken version av huvudmålservern installeras med hjälp av följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a45a1-287">To upgrade, select **Y**.  After the setup has been completed, check the version of the master target installed by using the following command.</span></span>

    ```
    cat /usr/local/.vx_version
    ```

<span data-ttu-id="a45a1-288">Kan du se att den **Version** fältet visar versionsnumret på Huvudmålet.</span><span class="sxs-lookup"><span data-stu-id="a45a1-288">You can see that the **Version** field gives the version number of the master target.</span></span>

### <a name="install-vmware-tools-on-the-master-target-server"></a><span data-ttu-id="a45a1-289">Installera VMware-verktyg på huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a45a1-289">Install VMware tools on the master target server</span></span>

<span data-ttu-id="a45a1-290">Du måste installera VMware-verktyg på huvudmålservern så att den kan identifiera datalager.</span><span class="sxs-lookup"><span data-stu-id="a45a1-290">You need to install VMware tools on the master target so that it can discover the data stores.</span></span> <span data-ttu-id="a45a1-291">Om inte verktygen är installerade, visas skyddar skärmen inte i datalager.</span><span class="sxs-lookup"><span data-stu-id="a45a1-291">If the tools are not installed, the reprotect screen isn't listed in the data stores.</span></span> <span data-ttu-id="a45a1-292">Du måste starta om efter installationen av VMware-verktyg.</span><span class="sxs-lookup"><span data-stu-id="a45a1-292">After installation of the VMware tools, you need to restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a45a1-293">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a45a1-293">Next steps</span></span>
<span data-ttu-id="a45a1-294">Efter installation och registrering av huvudmålservern har finsihed, du kan se huvudmålservern visas på den **Huvudmålservern** i avsnittet **Site Recovery-infrastruktur**, under Konfigurationsöversikt för servern.</span><span class="sxs-lookup"><span data-stu-id="a45a1-294">After the installation and registration of the master target has finsihed, you can see the master target appear on the **Master Target** section in **Site Recovery Infrastructure**, under the configuration server overview.</span></span>

<span data-ttu-id="a45a1-295">Du kan nu fortsätta med [återaktivera skydd](site-recovery-how-to-reprotect.md), följt av återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="a45a1-295">You can now proceed with [reprotection](site-recovery-how-to-reprotect.md), followed by failback.</span></span>

## <a name="common-issues"></a><span data-ttu-id="a45a1-296">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="a45a1-296">Common issues</span></span>

* <span data-ttu-id="a45a1-297">Kontrollera att du inte aktiverar Storage vMotion på alla management-komponenter, till exempel en huvudmålserver.</span><span class="sxs-lookup"><span data-stu-id="a45a1-297">Make sure you do not turn on Storage vMotion on any management components such as a master target.</span></span> <span data-ttu-id="a45a1-298">Om huvudmålservern flyttas efter en lyckad skyddar, kan virtuella diskar (VMDKs) inte frånkopplas.</span><span class="sxs-lookup"><span data-stu-id="a45a1-298">If the master target moves after a successful reprotect, the virtual machine disks (VMDKs) cannot be detached.</span></span> <span data-ttu-id="a45a1-299">I det här fallet misslyckas återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="a45a1-299">In this case, failback fails.</span></span>

* <span data-ttu-id="a45a1-300">Huvudmålservern ska inte ha några ögonblicksbilder på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a45a1-300">The master target should not have any snapshots on the virtual machine.</span></span> <span data-ttu-id="a45a1-301">Om det finns ögonblicksbilder, misslyckas återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="a45a1-301">If there are snapshots, failback fails.</span></span>

* <span data-ttu-id="a45a1-302">På grund av vissa anpassade NIC-konfigurationer med vissa kunder nätverksgränssnittet inaktiveras under start och huvudmålservern-agenten gick inte att initiera.</span><span class="sxs-lookup"><span data-stu-id="a45a1-302">Due to some custom NIC configurations at some customers, the network interface is disabled during startup, and the master target agent cannot initialize.</span></span> <span data-ttu-id="a45a1-303">Kontrollera att följande egenskaper är korrekt inställda.</span><span class="sxs-lookup"><span data-stu-id="a45a1-303">Make sure that the following properties are correctly set.</span></span> <span data-ttu-id="a45a1-304">Kontrollera de här egenskaperna i Ethernet-kort filens /etc/sysconfig/network-scripts/ifcfg-eth *.</span><span class="sxs-lookup"><span data-stu-id="a45a1-304">Check these properties in the Ethernet card file's /etc/sysconfig/network-scripts/ifcfg-eth*.</span></span>
    * <span data-ttu-id="a45a1-305">BOOTPROTO = dhcp</span><span class="sxs-lookup"><span data-stu-id="a45a1-305">BOOTPROTO=dhcp</span></span>
    * <span data-ttu-id="a45a1-306">ONBOOT = Ja</span><span class="sxs-lookup"><span data-stu-id="a45a1-306">ONBOOT=yes</span></span>
