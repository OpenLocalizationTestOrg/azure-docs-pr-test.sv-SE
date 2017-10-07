---
title: "aaaHow tooinstall en Linux-huvudmålserver för redundans från Azure tooon lokala | Microsoft Docs"
description: "Innan du skydda en Linux-dator, måste en Linux-huvudmålserver. Lär dig hur tooinstall en."
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
ms.openlocfilehash: d7c55d115712b9862414979f89efb1f177c5f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-linux-master-target-server"></a><span data-ttu-id="a722e-104">Installera en Linux-huvudmålsserver</span><span class="sxs-lookup"><span data-stu-id="a722e-104">Install a Linux master target server</span></span>
<span data-ttu-id="a722e-105">När du växlar över dina virtuella datorer kan du växla tillbaka hello virtuella datorer toohello lokal plats.</span><span class="sxs-lookup"><span data-stu-id="a722e-105">After you fail over your virtual machines, you can fail back hello virtual machines toohello on-premises site.</span></span> <span data-ttu-id="a722e-106">tillbaka toofail måste tooreprotect hello virtuell dator från Azure toohello lokal plats.</span><span class="sxs-lookup"><span data-stu-id="a722e-106">toofail back, you need tooreprotect hello virtual machine from Azure toohello on-premises site.</span></span> <span data-ttu-id="a722e-107">För den här processen behöver du en lokal huvudmålservern server tooreceive hello-trafik.</span><span class="sxs-lookup"><span data-stu-id="a722e-107">For this process, you need an on-premises master target server tooreceive hello traffic.</span></span> 

<span data-ttu-id="a722e-108">Om den skydda virtuella datorn är en Windows-dator, måste en Windows-huvudmålserver.</span><span class="sxs-lookup"><span data-stu-id="a722e-108">If your protected virtual machine is a Windows virtual machine, then you need a Windows master target.</span></span> <span data-ttu-id="a722e-109">För en virtuell Linux-dator behöver du en Linux-huvudmålserverns.</span><span class="sxs-lookup"><span data-stu-id="a722e-109">For a Linux virtual machine, you need a Linux master target.</span></span> <span data-ttu-id="a722e-110">Läs hello följande steg toolearn hur toocreate och installera en Linux master-mål.</span><span class="sxs-lookup"><span data-stu-id="a722e-110">Read hello following steps toolearn how toocreate and install a Linux master target.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a722e-111">Från och med lanseringen av hello 9.10.0 huvudmålservern kan hello senaste huvudmålservern endast installeras på en 16.04 Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="a722e-111">Starting with release of hello 9.10.0 master target server, hello latest master target server can be only installed on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="a722e-112">Nya installationer är inte tillåtna på CentOS6.6 servrar.</span><span class="sxs-lookup"><span data-stu-id="a722e-112">New installations aren't allowed on  CentOS6.6 servers.</span></span> <span data-ttu-id="a722e-113">Du kan dock fortsätta tooupgrade ditt gamla huvudnyckeln målservrar med hello 9.10.0 version.</span><span class="sxs-lookup"><span data-stu-id="a722e-113">However, you can continue tooupgrade your old master target servers by using hello 9.10.0 version.</span></span>

## <a name="overview"></a><span data-ttu-id="a722e-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="a722e-114">Overview</span></span>
<span data-ttu-id="a722e-115">Den här artikeln innehåller anvisningar för hur tooinstall en Linux master-mål.</span><span class="sxs-lookup"><span data-stu-id="a722e-115">This article provides instructions for how tooinstall a Linux master target.</span></span>

<span data-ttu-id="a722e-116">Skicka kommentarer eller frågor hello slutet av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="a722e-116">Post comments or questions at hello end of this article or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a722e-117">Krav</span><span class="sxs-lookup"><span data-stu-id="a722e-117">Prerequisites</span></span>

* <span data-ttu-id="a722e-118">toochoose hello värden på vilka toodeploy hello Huvudmålet, avgör om hello återställning kommer toobe tooan befintlig lokal virtuell dator eller tooa ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a722e-118">toochoose hello host on which toodeploy hello master target, determine if hello failback is going toobe tooan existing on-premises virtual machine or tooa new virtual machine.</span></span> 
    * <span data-ttu-id="a722e-119">Hello värden för hello huvudmålservern ska ha åtkomst toohello datalager för hello virtuell dator för en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a722e-119">For an existing virtual machine, hello host of hello master target should have access toohello data stores of hello virtual machine.</span></span>
    * <span data-ttu-id="a722e-120">Om hello lokala virtuella datorn inte finns, skapas hello återställning av virtuell dator på hello samma värden som hello huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a722e-120">If hello on-premises virtual machine does not exist, hello failback virtual machine is created on hello same host as hello master target.</span></span> <span data-ttu-id="a722e-121">Du kan välja valfri ESXi-värd tooinstall hello huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a722e-121">You can choose any ESXi host tooinstall hello master target.</span></span>
* <span data-ttu-id="a722e-122">Hej huvudmålservern ska vara i ett nätverk som kan kommunicera med hello processervern och konfigurationsservern hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-122">hello master target should be on a network that can communicate with hello process server and hello configuration server.</span></span>
* <span data-ttu-id="a722e-123">hello måste hello huvudmålservern vara lika tooor tidigare än hello versioner av hello processervern och konfigurationsservern hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-123">hello version of hello master target must be equal tooor earlier than hello versions of hello process server and hello configuration server.</span></span> <span data-ttu-id="a722e-124">Till exempel om hello version av hello konfigurationsservern 9.4 hello versionen av hello huvudmålservern kan vara 9.4 eller 9.3 men inte 9.5.</span><span class="sxs-lookup"><span data-stu-id="a722e-124">For example, if hello version of hello configuration server is 9.4, hello version of hello master target can be 9.4 or 9.3 but not 9.5.</span></span>
* <span data-ttu-id="a722e-125">Hej huvudmålservern kan bara finnas en virtuell VMware-dator och inte en fysisk server.</span><span class="sxs-lookup"><span data-stu-id="a722e-125">hello master target can only be a VMware virtual machine and not a physical server.</span></span>

## <a name="create-hello-master-target-according-toohello-sizing-guidelines"></a><span data-ttu-id="a722e-126">Skapa hello huvudmålservern enligt toohello storlek riktlinjer</span><span class="sxs-lookup"><span data-stu-id="a722e-126">Create hello master target according toohello sizing guidelines</span></span>

<span data-ttu-id="a722e-127">Skapa hello huvudmålservern i enlighet med hello följa riktlinjerna för storlek:</span><span class="sxs-lookup"><span data-stu-id="a722e-127">Create hello master target in accordance with hello following sizing guidelines:</span></span>
- <span data-ttu-id="a722e-128">**RAM-minne**: 6 GB eller mer</span><span class="sxs-lookup"><span data-stu-id="a722e-128">**RAM**: 6 GB or more</span></span>
- <span data-ttu-id="a722e-129">**OS-diskstorlek**: 100 GB eller mer (tooinstall CentOS6.6)</span><span class="sxs-lookup"><span data-stu-id="a722e-129">**OS disk size**: 100 GB or more (tooinstall CentOS6.6)</span></span>
- <span data-ttu-id="a722e-130">**Ytterligare diskstorleken för kvarhållningsenhetens**: 1 TB</span><span class="sxs-lookup"><span data-stu-id="a722e-130">**Additional disk size for retention drive**: 1 TB</span></span>
- <span data-ttu-id="a722e-131">**CPU-kärnor**: 4 kärnor eller mer</span><span class="sxs-lookup"><span data-stu-id="a722e-131">**CPU cores**: 4 cores or more</span></span>

<span data-ttu-id="a722e-132">hello följande stöd för Ubuntu kärnor stöds.</span><span class="sxs-lookup"><span data-stu-id="a722e-132">hello following supported Ubuntu kernels are supported.</span></span>


|<span data-ttu-id="a722e-133">Kernel-serien</span><span class="sxs-lookup"><span data-stu-id="a722e-133">Kernel Series</span></span>  |<span data-ttu-id="a722e-134">Stöd för upp</span><span class="sxs-lookup"><span data-stu-id="a722e-134">Support up too</span></span> |
|---------|---------|
|<span data-ttu-id="a722e-135">4.4</span><span class="sxs-lookup"><span data-stu-id="a722e-135">4.4</span></span>      |<span data-ttu-id="a722e-136">4.4.0-81-Generic</span><span class="sxs-lookup"><span data-stu-id="a722e-136">4.4.0-81-generic</span></span>         |
|<span data-ttu-id="a722e-137">4.8</span><span class="sxs-lookup"><span data-stu-id="a722e-137">4.8</span></span>      |<span data-ttu-id="a722e-138">4.8.0-56-Generic</span><span class="sxs-lookup"><span data-stu-id="a722e-138">4.8.0-56-generic</span></span>         |
|<span data-ttu-id="a722e-139">4.10</span><span class="sxs-lookup"><span data-stu-id="a722e-139">4.10</span></span>     |<span data-ttu-id="a722e-140">4.10.0-24-Generic</span><span class="sxs-lookup"><span data-stu-id="a722e-140">4.10.0-24-generic</span></span>        |


## <a name="deploy-hello-master-target-server"></a><span data-ttu-id="a722e-141">Distribuera hello huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a722e-141">Deploy hello master target server</span></span>

### <a name="install-ubuntu-16042-minimal"></a><span data-ttu-id="a722e-142">Installera Ubuntu 16.04.2 Minimal</span><span class="sxs-lookup"><span data-stu-id="a722e-142">Install Ubuntu 16.04.2 Minimal</span></span>

<span data-ttu-id="a722e-143">Ta hello följande hello steg tooinstall hello Ubuntu 16.04.2 64-bitars operativsystem.</span><span class="sxs-lookup"><span data-stu-id="a722e-143">Take hello following hello steps tooinstall hello Ubuntu 16.04.2 64-bit operating system.</span></span>

<span data-ttu-id="a722e-144">**Steg 1:** gå toohello [Hämta länk](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) och välj hello närmaste spegling från som hämtar en Ubuntu 16.04.2 minimal 64-bitars ISO.</span><span class="sxs-lookup"><span data-stu-id="a722e-144">**Step 1:** Go toohello [download link](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) and choose hello closest mirror from which download an Ubuntu 16.04.2 minimal 64-bit ISO.</span></span>

<span data-ttu-id="a722e-145">Behåll en Ubuntu 16.04.2 minimal 64-bitars ISO i hello DVD-enheten och starta hello system.</span><span class="sxs-lookup"><span data-stu-id="a722e-145">Keep an Ubuntu 16.04.2 minimal 64-bit ISO in hello DVD drive and start hello system.</span></span>

<span data-ttu-id="a722e-146">**Steg 2:** Välj **engelska** som önskat språk och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-146">**Step 2:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Välj ett språk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

<span data-ttu-id="a722e-148">**Steg 3:** Välj **installerar Ubuntu Server**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-148">**Step 3:** Select **Install Ubuntu Server**, and then select **Enter**.</span></span>

![Välj installerar Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

<span data-ttu-id="a722e-150">**Steg 4:** Välj **engelska** som önskat språk och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-150">**Step 4:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Välj engelska som ditt språk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

<span data-ttu-id="a722e-152">**Steg 5:** Välj hello lämpligt alternativ från hello **tidszon** alternativ, och välj **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-152">**Step 5:** Select hello appropriate option from hello **Time Zone** options list, and then select **Enter**.</span></span>

![Välj hello tidszon](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

<span data-ttu-id="a722e-154">**Steg 6:** Välj **nr** (hello standardalternativet), och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-154">**Step 6:** Select **No** (hello default option), and then select **Enter**.</span></span>


![Konfigurera hello tangentbord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

<span data-ttu-id="a722e-156">**Steg 7:** Välj **engelska (USA)** som hello land för hello tangentbord och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-156">**Step 7:** Select **English (US)** as hello country of origin for hello keyboard, and then select **Enter**.</span></span>

![Välj USA som hello land för ursprung](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

<span data-ttu-id="a722e-158">**Steg 8:** Välj **engelska (USA)** som hello tangentbordslayout och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-158">**Step 8:** Select **English (US)** as hello keyboard layout, and then select **Enter**.</span></span>

![Välj amerikansk engelska som hello tangentbordslayout](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

<span data-ttu-id="a722e-160">**Steg 9:** ange hello värdnamn för servern i hello **värdnamn** och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a722e-160">**Step 9:** Enter hello hostname for your server in hello **Hostname** box, and then select **Continue**.</span></span>

![Ange hello värdnamn för servern](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

<span data-ttu-id="a722e-162">**Steg 10:** toocreate ett användarkonto, ange hello användarnamn och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a722e-162">**Step 10:** toocreate a user account, enter hello user name, and then select **Continue**.</span></span>

![Skapa ett användarkonto](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

<span data-ttu-id="a722e-164">**Steg 11:** ange hello lösenord för hello nytt användarkonto och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a722e-164">**Step 11:** Enter hello password for hello new user account, and then select **Continue**.</span></span>

![Ange hello lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

<span data-ttu-id="a722e-166">**Steg 12:** bekräfta hello lösenord för hello nya användaren och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a722e-166">**Step 12:** Confirm hello password for hello new user, and then select **Continue**.</span></span>

![Bekräfta hello lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

<span data-ttu-id="a722e-168">**Steg 13:** Välj **nr** (hello standardalternativet), och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-168">**Step 13:** Select **No** (hello default option), and then select **Enter**.</span></span>

![Konfigurera användare och lösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

<span data-ttu-id="a722e-170">**Steg 14:** om hello tidszon som visas är korrekt, Välj **Ja** (hello standardalternativet), och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-170">**Step 14:** If hello time zone that's displayed is correct, select **Yes** (hello default option), and then select **Enter**.</span></span>

<span data-ttu-id="a722e-171">tooreconfigure din tidszon, Välj **nr**.</span><span class="sxs-lookup"><span data-stu-id="a722e-171">tooreconfigure your time zone, select **No**.</span></span>

![Konfigurera hello klockan](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

<span data-ttu-id="a722e-173">**Steg 15:** hello partitionering metoden alternativ, Välj **interaktiv - använder hela disken**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-173">**Step 15:** From hello partitioning method options, select **Guided - use entire disk**, and then select **Enter**.</span></span>

![Välj hello partitionering metodalternativet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

<span data-ttu-id="a722e-175">**Steg 16:** Välj hello lämplig disk från hello **Välj disk toopartition** alternativ och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-175">**Step 16:** Select hello appropriate disk from hello **Select disk toopartition** options, and then select **Enter**.</span></span>


![Välj hello disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

<span data-ttu-id="a722e-177">**Steg 17:** Välj **Ja** toowrite hello ändringar toodisk och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-177">**Step 17:** Select **Yes** toowrite hello changes toodisk, and then select **Enter**.</span></span>

![Skriva hello ändringar toodisk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

<span data-ttu-id="a722e-179">**Steg 18:** väljer hello standardalternativet, väljer **Fortsätt**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-179">**Step 18:** Select hello default option, select **Continue**, and then select **Enter**.</span></span>

![Välj hello standardalternativet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

<span data-ttu-id="a722e-181">**Steg 19:** Välj hello lämpligt alternativ för att hantera uppgraderingar på datorn och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-181">**Step 19:** Select hello appropriate option for managing upgrades on your system, and then select **Enter**.</span></span>

![Välj hur toomanage uppgraderas](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> <span data-ttu-id="a722e-183">Eftersom Azure Site Recovery hello huvudmålservern kräver en särskild version av hello Ubuntu, behöver du tooensure som hello kernel uppgraderingar har inaktiverats för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a722e-183">Because hello Azure Site Recovery master target server requires a very specific version of hello Ubuntu, you need tooensure that hello kernel upgrades are disabled for hello virtual machine.</span></span> <span data-ttu-id="a722e-184">Om de är aktiverade kan orsaka reguljära uppgraderingar hello huvudmålservern server toomalfunction.</span><span class="sxs-lookup"><span data-stu-id="a722e-184">If they are enabled, then any regular upgrades cause hello master target server toomalfunction.</span></span> <span data-ttu-id="a722e-185">Kontrollera att du väljer hello **inga automatiska uppdateringar** alternativet.</span><span class="sxs-lookup"><span data-stu-id="a722e-185">Make sure you select hello **No automatic updates** option.</span></span>


<span data-ttu-id="a722e-186">**Steg 20:** Välj standardalternativen.</span><span class="sxs-lookup"><span data-stu-id="a722e-186">**Step 20:** Select default options.</span></span> <span data-ttu-id="a722e-187">Om du vill openSSH för SSH ansluta, Välj hello **OpenSSH server** alternativet och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="a722e-187">If you want openSSH for SSH connect, select hello **OpenSSH server** option, and then select **Continue**.</span></span>

![Välj program](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

<span data-ttu-id="a722e-189">**Steg 21:** Välj **Ja**, och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-189">**Step 21:** Select **Yes**, and then select **Enter**.</span></span>

![Startprogram för installationen hello GRUB](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

<span data-ttu-id="a722e-191">**Steg 22:** Välj hello lämplig enhet för hello Start inläsaren installation (helst **/dev/sda**), och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="a722e-191">**Step 22:** Select hello appropriate device for hello boot loader installation (preferably **/dev/sda**), and then select **Enter**.</span></span>

![Välj en enhet för start inläsaren installation](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

<span data-ttu-id="a722e-193">**Steg 23:** Välj **Fortsätt**, och välj sedan **RETUR** toofinish hello installation.</span><span class="sxs-lookup"><span data-stu-id="a722e-193">**Step 23:** Select **Continue**, and then select **Enter** toofinish hello installation.</span></span>

![Slut hello installationen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

<span data-ttu-id="a722e-195">När hello installationen är klar, logga in toohello VM med hello nya autentiseringsuppgifterna för användaren.</span><span class="sxs-lookup"><span data-stu-id="a722e-195">After hello installation has finished, sign in toohello VM with hello new user credentials.</span></span> <span data-ttu-id="a722e-196">(Se för**steg 10** för mer information.)</span><span class="sxs-lookup"><span data-stu-id="a722e-196">(Refer too**Step 10** for more information.)</span></span>

<span data-ttu-id="a722e-197">Ta hello stegen som beskrivs i följande skärmbild tooset hello rot hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="a722e-197">Take hello steps that are described in hello following screenshot tooset hello ROOT user password.</span></span> <span data-ttu-id="a722e-198">Logga in som rotanvändare.</span><span class="sxs-lookup"><span data-stu-id="a722e-198">Then sign in as ROOT user.</span></span>

![Ange hello rot användarlösenord](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-hello-machine-for-configuration-as-a-master-target-server"></a><span data-ttu-id="a722e-200">Förbered hello datorn för konfigurationen som en huvudmålserver</span><span class="sxs-lookup"><span data-stu-id="a722e-200">Prepare hello machine for configuration as a master target server</span></span>
<span data-ttu-id="a722e-201">Förbered hello datorn för konfigurationen som en huvudmålserver.</span><span class="sxs-lookup"><span data-stu-id="a722e-201">Next, prepare hello machine for configuration as a master target server.</span></span>

<span data-ttu-id="a722e-202">tooget hello-ID för varje SCSI-hårddisk i en virtuell Linux-dator, aktivera hello **disk. EnableUUID = TRUE** parameter.</span><span class="sxs-lookup"><span data-stu-id="a722e-202">tooget hello ID for each SCSI hard disk in a Linux virtual machine, enable hello **disk.EnableUUID = TRUE** parameter.</span></span>

<span data-ttu-id="a722e-203">tooenable den här parametern tar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a722e-203">tooenable this parameter, take hello following steps:</span></span>

1. <span data-ttu-id="a722e-204">Stäng av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a722e-204">Shut down your virtual machine.</span></span>

2. <span data-ttu-id="a722e-205">Högerklicka hello posten för hello virtuell dator i hello till vänster och välj sedan **redigera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="a722e-205">Right-click hello entry for hello virtual machine in hello left pane, and then select **Edit Settings**.</span></span>

3. <span data-ttu-id="a722e-206">Välj hello **alternativ** fliken.</span><span class="sxs-lookup"><span data-stu-id="a722e-206">Select hello **Options** tab.</span></span>

4. <span data-ttu-id="a722e-207">I hello vänster och välj **Avancerat** > **allmänna**, och välj sedan hello **konfigurationsparametrar** knappen hello nedre högra tillhör hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="a722e-207">In hello left pane, select **Advanced** > **General**, and then select hello **Configuration Parameters** button on hello lower-right part of hello screen.</span></span>

    ![Alternativfliken](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    <span data-ttu-id="a722e-209">Hej **konfigurationsparametrar** alternativet är inte tillgängligt när hello datorn körs.</span><span class="sxs-lookup"><span data-stu-id="a722e-209">hello **Configuration Parameters** option is not available when hello machine is running.</span></span> <span data-ttu-id="a722e-210">toomake på den här fliken är aktiv, Stäng hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a722e-210">toomake this tab active, shut down hello virtual machine.</span></span>

5. <span data-ttu-id="a722e-211">Se om en rad med **disk. EnableUUID** finns redan.</span><span class="sxs-lookup"><span data-stu-id="a722e-211">See whether a row with **disk.EnableUUID** already exists.</span></span>

    - <span data-ttu-id="a722e-212">Om värdet för hello finns och har angetts för**FALSKT**, ändrar hello-värdet för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="a722e-212">If hello value exists and is set too**False**, change hello value too**True**.</span></span> <span data-ttu-id="a722e-213">(hello värden är inte skiftlägeskänsligt.)</span><span class="sxs-lookup"><span data-stu-id="a722e-213">(hello values are not case-sensitive.)</span></span>

    - <span data-ttu-id="a722e-214">Om värdet för hello finns och har angetts för**SANT**väljer **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="a722e-214">If hello value exists and is set too**True**, select **Cancel**.</span></span>

    - <span data-ttu-id="a722e-215">Om hello värdet inte finns, väljer **Lägg till rad**.</span><span class="sxs-lookup"><span data-stu-id="a722e-215">If hello value does not exist, select **Add Row**.</span></span>

    - <span data-ttu-id="a722e-216">Hello namnkolumnen lägga till **disk. EnableUUID**, och sedan ange hello värdet för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="a722e-216">In hello name column, add **disk.EnableUUID**, and then set hello value too**TRUE**.</span></span>

    ![Kontrollerar om disken. EnableUUID finns redan](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a><span data-ttu-id="a722e-218">Inaktivera kernel-uppgraderingar</span><span class="sxs-lookup"><span data-stu-id="a722e-218">Disable kernel upgrades</span></span>

<span data-ttu-id="a722e-219">Azure Site Recovery-huvudmålservern kräver en särskild version av hello Ubuntu, se till att hello kernel uppgraderingar är inaktiverat för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a722e-219">Azure Site Recovery master target server requires a very specific version of hello Ubuntu, ensure that hello kernel upgrades are disabled for hello virtual machine.</span></span>

<span data-ttu-id="a722e-220">Om kernel uppgraderingar är aktiverade kan orsaka reguljära uppgraderingar hello huvudmålservern server toomalfunction.</span><span class="sxs-lookup"><span data-stu-id="a722e-220">If kernel upgrades are enabled, then any regular upgrades cause hello master target server toomalfunction.</span></span>

#### <a name="download-and-install-additional-packages"></a><span data-ttu-id="a722e-221">Hämta och installera ytterligare paket</span><span class="sxs-lookup"><span data-stu-id="a722e-221">Download and install additional packages</span></span>

> [!NOTE]
> <span data-ttu-id="a722e-222">Kontrollera att du har toodownload för Internet-anslutning och installera ytterligare paket.</span><span class="sxs-lookup"><span data-stu-id="a722e-222">Make sure that you have Internet connectivity toodownload and install additional packages.</span></span> <span data-ttu-id="a722e-223">Om du inte har anslutning till Internet, måste toomanually hitta dessa RPM-paket och installera dem.</span><span class="sxs-lookup"><span data-stu-id="a722e-223">If you don't have Internet connectivity, you need toomanually find these RPM packages and install them.</span></span>

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-hello-installer-for-setup"></a><span data-ttu-id="a722e-224">Hämta hello installer för installationen</span><span class="sxs-lookup"><span data-stu-id="a722e-224">Get hello installer for setup</span></span>

<span data-ttu-id="a722e-225">Om din huvudmålservern har Internetanslutning, kan du använda följande steg toodownload hello installer hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-225">If your master target has Internet connectivity, you can use hello following steps toodownload hello installer.</span></span> <span data-ttu-id="a722e-226">Annars kan du kopiera hello installationsprogrammet från hello processervern och installera den.</span><span class="sxs-lookup"><span data-stu-id="a722e-226">Otherwise, you can copy hello installer from hello process server and then install it.</span></span>

#### <a name="download-hello-master-target-installation-packages"></a><span data-ttu-id="a722e-227">Hämta hello huvudmålservern-paket</span><span class="sxs-lookup"><span data-stu-id="a722e-227">Download hello master target installation packages</span></span>

<span data-ttu-id="a722e-228">[Hämta hello senaste Linux huvudmålservern installation bits](https://aka.ms/latestlinuxmobsvc).</span><span class="sxs-lookup"><span data-stu-id="a722e-228">[Download hello latest Linux master target installation bits](https://aka.ms/latestlinuxmobsvc).</span></span>

<span data-ttu-id="a722e-229">toodownload den med hjälp av Linux, typ:</span><span class="sxs-lookup"><span data-stu-id="a722e-229">toodownload it by using Linux, type:</span></span>

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

<span data-ttu-id="a722e-230">Kontrollera att du hämtar och packa hello installer i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="a722e-230">Make sure that you download and unzip hello installer in your home directory.</span></span> <span data-ttu-id="a722e-231">Om du packa för**/usr/lokal**, misslyckas hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="a722e-231">If you unzip too**/usr/Local**, then hello installation  fails.</span></span>


#### <a name="access-hello-installer-from-hello-process-server"></a><span data-ttu-id="a722e-232">Åtkomst hello installer från hello processervern</span><span class="sxs-lookup"><span data-stu-id="a722e-232">Access hello installer from hello process server</span></span>

1. <span data-ttu-id="a722e-233">Hej processervern gå för**C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span><span class="sxs-lookup"><span data-stu-id="a722e-233">On hello process server, go too**C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span></span>

2. <span data-ttu-id="a722e-234">Kopiera hello krävs installer-fil från hello processervern och sparar den som **latestlinuxmobsvc.tar.gz** i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="a722e-234">Copy hello required installer file from hello process server, and save it as **latestlinuxmobsvc.tar.gz** in your home directory.</span></span>


### <a name="apply-custom-configuration-changes"></a><span data-ttu-id="a722e-235">Använd anpassad konfigurationsändringar</span><span class="sxs-lookup"><span data-stu-id="a722e-235">Apply custom configuration changes</span></span>

<span data-ttu-id="a722e-236">tooapply anpassade konfigurationsändringar, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a722e-236">tooapply custom configuration changes, use hello following steps:</span></span>


1. <span data-ttu-id="a722e-237">Kör hello efter kommandot toountar hello binary.</span><span class="sxs-lookup"><span data-stu-id="a722e-237">Run hello following command toountar hello binary.</span></span>
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Skärmbild av hello kommandot toorun](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. <span data-ttu-id="a722e-239">Kör följande kommando toogive behörighet hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-239">Run hello following command toogive permission.</span></span>
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. <span data-ttu-id="a722e-240">Kör följande kommandoskript toorun hello hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-240">Run hello following command toorun hello script.</span></span>
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> <span data-ttu-id="a722e-241">Kör hello skriptet en gång på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="a722e-241">Run hello script only once on hello server.</span></span> <span data-ttu-id="a722e-242">Stäng hello-servern.</span><span class="sxs-lookup"><span data-stu-id="a722e-242">Shut down hello server.</span></span> <span data-ttu-id="a722e-243">Starta sedan om hello server när du lägger till en disk, enligt beskrivningen i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-243">Then restart hello server after you add a disk, as described in hello next section.</span></span>

### <a name="add-a-retention-disk-toohello-linux-master-target-virtual-machine"></a><span data-ttu-id="a722e-244">Lägg till en kvarhållning disk toohello Linux huvudmålservern virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a722e-244">Add a retention disk toohello Linux master target virtual machine</span></span>

<span data-ttu-id="a722e-245">Använd följande steg toocreate en kvarhållningsdisken hello:</span><span class="sxs-lookup"><span data-stu-id="a722e-245">Use hello following steps toocreate a retention disk:</span></span>

1. <span data-ttu-id="a722e-246">Koppla en ny virtuella för 1 TB-disk toohello Linux huvudmålservern och starta sedan hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="a722e-246">Attach a new 1-TB disk toohello Linux master target virtual machine, and then start hello machine.</span></span>

2. <span data-ttu-id="a722e-247">Använd hello **multipath -lla** kommando toolearn hello multipath-ID för hello kvarhållningsdisken.</span><span class="sxs-lookup"><span data-stu-id="a722e-247">Use hello **multipath -ll** command toolearn hello multipath ID of hello retention disk.</span></span>

    ```
    multipath -ll
    ```
    ![hello multipath ID hello kvarhållningsdisken](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. <span data-ttu-id="a722e-249">Formatera hello enhet och sedan skapa ett filsystem i hello ny enhet.</span><span class="sxs-lookup"><span data-stu-id="a722e-249">Format hello drive, and then create a file system on hello new drive.</span></span>

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Skapa ett filsystem på hello-enhet](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. <span data-ttu-id="a722e-251">När du har skapat hello filsystemet montera hello kvarhållningsdisken.</span><span class="sxs-lookup"><span data-stu-id="a722e-251">After you create hello file system, mount hello retention disk.</span></span>
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Montering hello kvarhållningsdisken](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. <span data-ttu-id="a722e-253">Skapa hello **fstab** post toomount hello kvarhållningsenhetens varje gång hello systemet startar.</span><span class="sxs-lookup"><span data-stu-id="a722e-253">Create hello **fstab** entry toomount hello retention drive every time hello system starts.</span></span>
    ```
    vi /etc/fstab
    ```
    <span data-ttu-id="a722e-254">Välj **infoga** toobegin hello Filredigering.</span><span class="sxs-lookup"><span data-stu-id="a722e-254">Select **Insert** toobegin editing hello file.</span></span> <span data-ttu-id="a722e-255">Skapa en ny rad och infoga hello efter texten.</span><span class="sxs-lookup"><span data-stu-id="a722e-255">Create a new line, and then insert hello following text.</span></span> <span data-ttu-id="a722e-256">Redigera hello disk multipath ID baserat på hello markerat multipath ID från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="a722e-256">Edit hello disk multipath ID based on hello highlighted multipath ID from hello previous command.</span></span>

    <span data-ttu-id="a722e-257"> **/dev/mapper/ <Retention disks multipath id> /mnt/kvarhållning ext4 rw 0 0**</span><span class="sxs-lookup"><span data-stu-id="a722e-257">**/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**</span></span>

    <span data-ttu-id="a722e-258">Välj **Esc**, och skriv sedan **: wq** (skriva och avsluta) tooclose hello editor-fönstret.</span><span class="sxs-lookup"><span data-stu-id="a722e-258">Select **Esc**, and then type **:wq** (write and quit) tooclose hello editor window.</span></span>

### <a name="install-hello-master-target"></a><span data-ttu-id="a722e-259">Installera hello huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a722e-259">Install hello master target</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a722e-260">hello måste hello huvudmålservern vara lika tooor tidigare än hello versioner av hello processervern och konfigurationsservern hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-260">hello version of hello master target server must be equal tooor earlier than hello versions of hello process server and hello configuration server.</span></span> <span data-ttu-id="a722e-261">Om detta inte är uppfyllt, skyddar lyckas, men kan slutföras inte.</span><span class="sxs-lookup"><span data-stu-id="a722e-261">If this condition is not met, reprotect succeeds, but replication fails.</span></span>


> [!NOTE]
> <span data-ttu-id="a722e-262">Kontrollera att hello innan du installerar hello huvudmålservern **/etc/hosts** filen på hello virtuell dator innehåller poster som mappar hello lokala värdnamnet toohello IP-adresser som är associerade med alla nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="a722e-262">Before you install hello master target server, check that hello **/etc/hosts** file on hello virtual machine contains entries that map hello local hostname toohello IP addresses that are associated with all network adapters.</span></span>

1. <span data-ttu-id="a722e-263">Kopiera hello lösenfras från **C:\ProgramData\Microsoft Azure plats Recovery\private\connection.passphrase** på hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a722e-263">Copy hello passphrase from **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** on hello configuration server.</span></span> <span data-ttu-id="a722e-264">Spara den som **passphrase.txt** i hello hello samma lokala katalog genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a722e-264">Then save it as **passphrase.txt** in hello same local directory by running hello following command:</span></span>

    ```
    echo <passphrase> >passphrase.txt
    ```
    <span data-ttu-id="a722e-265">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a722e-265">Example:</span></span> 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. <span data-ttu-id="a722e-266">Observera hello configuration serverns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a722e-266">Note hello configuration server's IP address.</span></span> <span data-ttu-id="a722e-267">Du behöver det i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a722e-267">You need it in hello next step.</span></span>

3. <span data-ttu-id="a722e-268">Kör följande kommando tooinstall hello huvudmålservern hello och registrera hello-servern med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a722e-268">Run hello following command tooinstall hello master target server and register hello server with hello configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    <span data-ttu-id="a722e-269">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a722e-269">Example:</span></span> 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    <span data-ttu-id="a722e-270">Vänta tills hello skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="a722e-270">Wait until hello script finishes.</span></span> <span data-ttu-id="a722e-271">Om hello huvudmålservern registrerar konfigurationsändringarna, hello huvudmålservern visas på hello **Site Recovery-infrastruktur** sidan hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a722e-271">If hello master target registers sucessfully, hello master target is listed on hello **Site Recovery Infrastructure** page of hello portal.</span></span>


#### <a name="install-hello-master-target-by-using-interactive-installation"></a><span data-ttu-id="a722e-272">Installera hello huvudmålservern genom att använda interaktiv installation</span><span class="sxs-lookup"><span data-stu-id="a722e-272">Install hello master target by using interactive installation</span></span>

1. <span data-ttu-id="a722e-273">Kör följande kommando tooinstall hello huvudmålservern hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-273">Run hello following command tooinstall hello master target.</span></span> <span data-ttu-id="a722e-274">Hello agent roll, Välj **Huvudmålservern**.</span><span class="sxs-lookup"><span data-stu-id="a722e-274">For hello agent role, choose **Master Target**.</span></span>

    ```
    ./install
    ```

2. <span data-ttu-id="a722e-275">Välj hello standardplatsen för installation och välj sedan **RETUR** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="a722e-275">Choose hello default location for installation, and then select **Enter** toocontinue.</span></span>

    ![Om du väljer en standardplatsen för installation av huvudmålservern](./media/site-recovery-how-to-install-linux-master-target/image17.png)

<span data-ttu-id="a722e-277">Registrera hello konfigurationsservern med hjälp av kommandoraden hello efter hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a722e-277">After hello installation has finished, register hello configuration server by using hello command line.</span></span>

1. <span data-ttu-id="a722e-278">Observera hello hello configuration serverns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a722e-278">Note hello IP address of hello configuration server.</span></span> <span data-ttu-id="a722e-279">Du behöver det i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a722e-279">You need it in hello next step.</span></span>

2. <span data-ttu-id="a722e-280">Kör följande kommando tooinstall hello huvudmålservern hello och registrera hello-servern med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a722e-280">Run hello following command tooinstall hello master target server and register hello server with hello configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    <span data-ttu-id="a722e-281">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a722e-281">Example:</span></span> 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   <span data-ttu-id="a722e-282">Vänta tills hello skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="a722e-282">Wait until hello script finishes.</span></span> <span data-ttu-id="a722e-283">Om hello huvudmålservern är registrerade har, hello huvudmålservern finns på hello **Site Recovery-infrastruktur** sidan hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a722e-283">If hello master target is registered succesfully, hello master target is listed on hello **Site Recovery Infrastructure** page of hello portal.</span></span>


### <a name="upgrade-hello-master-target"></a><span data-ttu-id="a722e-284">Uppgradera hello huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a722e-284">Upgrade hello master target</span></span>

<span data-ttu-id="a722e-285">Kör installationsprogrammet för hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-285">Run hello installer.</span></span> <span data-ttu-id="a722e-286">Den identifierar automatiskt den hello-agenten är installerad på hello Huvudmålet.</span><span class="sxs-lookup"><span data-stu-id="a722e-286">It automatically detects that hello agent is installed on hello master target.</span></span> <span data-ttu-id="a722e-287">tooupgrade, Välj **Y**.  När hello-installationen har slutförts, kontrollera hello version av hello huvudmålservern installeras med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="a722e-287">tooupgrade, select **Y**.  After hello setup has been completed, check hello version of hello master target installed by using hello following command.</span></span>

    ```
    cat /usr/local/.vx_version
    ```

<span data-ttu-id="a722e-288">Du kan se att hello **Version** fältet ger hello versionsnumret för hello huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="a722e-288">You can see that hello **Version** field gives hello version number of hello master target.</span></span>

### <a name="install-vmware-tools-on-hello-master-target-server"></a><span data-ttu-id="a722e-289">Installera VMware-verktyg på hello huvudmålservern</span><span class="sxs-lookup"><span data-stu-id="a722e-289">Install VMware tools on hello master target server</span></span>

<span data-ttu-id="a722e-290">Du måste tooinstall VMware-verktyg på hello Huvudmålet så att den kan identifiera hello datalager.</span><span class="sxs-lookup"><span data-stu-id="a722e-290">You need tooinstall VMware tools on hello master target so that it can discover hello data stores.</span></span> <span data-ttu-id="a722e-291">Om hello tools inte är installerat, visas skyddar hello-skärmen inte i hello datalager.</span><span class="sxs-lookup"><span data-stu-id="a722e-291">If hello tools are not installed, hello reprotect screen isn't listed in hello data stores.</span></span> <span data-ttu-id="a722e-292">Efter installationen av hello VMware-verktyg behöver du toorestart.</span><span class="sxs-lookup"><span data-stu-id="a722e-292">After installation of hello VMware tools, you need toorestart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a722e-293">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a722e-293">Next steps</span></span>
<span data-ttu-id="a722e-294">När hello installation och registrering av hello huvudmålservern har finsihed, kan du se hello huvudmålservern visas på hello **Huvudmålservern** i avsnittet **Site Recovery-infrastruktur**, under hello Översikt över konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a722e-294">After hello installation and registration of hello master target has finsihed, you can see hello master target appear on hello **Master Target** section in **Site Recovery Infrastructure**, under hello configuration server overview.</span></span>

<span data-ttu-id="a722e-295">Du kan nu fortsätta med [återaktivera skydd](site-recovery-how-to-reprotect.md), följt av återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="a722e-295">You can now proceed with [reprotection](site-recovery-how-to-reprotect.md), followed by failback.</span></span>

## <a name="common-issues"></a><span data-ttu-id="a722e-296">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="a722e-296">Common issues</span></span>

* <span data-ttu-id="a722e-297">Kontrollera att du inte aktiverar Storage vMotion på alla management-komponenter, till exempel en huvudmålserver.</span><span class="sxs-lookup"><span data-stu-id="a722e-297">Make sure you do not turn on Storage vMotion on any management components such as a master target.</span></span> <span data-ttu-id="a722e-298">Om hello huvudmålservern flyttas efter en lyckad skyddar, kan hello virtuella diskar (VMDKs) inte frånkopplas.</span><span class="sxs-lookup"><span data-stu-id="a722e-298">If hello master target moves after a successful reprotect, hello virtual machine disks (VMDKs) cannot be detached.</span></span> <span data-ttu-id="a722e-299">I det här fallet misslyckas återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="a722e-299">In this case, failback fails.</span></span>

* <span data-ttu-id="a722e-300">hello huvudmålservern ska inte ha några ögonblicksbilder på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a722e-300">hello master target should not have any snapshots on hello virtual machine.</span></span> <span data-ttu-id="a722e-301">Om det finns ögonblicksbilder, misslyckas återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="a722e-301">If there are snapshots, failback fails.</span></span>

* <span data-ttu-id="a722e-302">På grund av toosome anpassade NIC konfigurationer med vissa kunder hello nätverksgränssnittet inaktiveras under start och hello huvudmålservern agent kan inte initieras.</span><span class="sxs-lookup"><span data-stu-id="a722e-302">Due toosome custom NIC configurations at some customers, hello network interface is disabled during startup, and hello master target agent cannot initialize.</span></span> <span data-ttu-id="a722e-303">Kontrollera att hello följande egenskaper är korrekt inställda.</span><span class="sxs-lookup"><span data-stu-id="a722e-303">Make sure that hello following properties are correctly set.</span></span> <span data-ttu-id="a722e-304">Kontrollera de här egenskaperna i hello Ethernet-kort filens /etc/sysconfig/network-scripts/ifcfg-eth *.</span><span class="sxs-lookup"><span data-stu-id="a722e-304">Check these properties in hello Ethernet card file's /etc/sysconfig/network-scripts/ifcfg-eth*.</span></span>
    * <span data-ttu-id="a722e-305">BOOTPROTO = dhcp</span><span class="sxs-lookup"><span data-stu-id="a722e-305">BOOTPROTO=dhcp</span></span>
    * <span data-ttu-id="a722e-306">ONBOOT = Ja</span><span class="sxs-lookup"><span data-stu-id="a722e-306">ONBOOT=yes</span></span>
