---
title: "aaaProvision virtuella StorSimple-matris på VMware | Microsoft Docs"
description: "Den här andra kursen i virtuella StorSimple-matris deployment serien innebär att etablera en virtuell enhet i VMware."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0912c1c315a04ea46b6373a8fcd5554ecae14e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a><span data-ttu-id="56b4d-103">Distribuera StorSimple virtuell matris - etablera i VMware</span><span class="sxs-lookup"><span data-stu-id="56b4d-103">Deploy StorSimple Virtual Array - Provision in VMware</span></span>
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a><span data-ttu-id="56b4d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="56b4d-104">Overview</span></span>
<span data-ttu-id="56b4d-105">Den här självstudiekursen beskriver hur tooprovision och ansluta tooa virtuella StorSimple-matris på ett värdsystem som kör VMware ESXi 5.5 och senare.</span><span class="sxs-lookup"><span data-stu-id="56b4d-105">This tutorial describes how tooprovision and connect tooa StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span> <span data-ttu-id="56b4d-106">Den här artikeln gäller toohello distribution av virtuella StorSimple-matriser i Azure-portalen och hello Microsoft Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-106">This article applies toohello deployment of StorSimple Virtual Arrays in Azure portal and hello Microsoft Azure Government Cloud.</span></span>

<span data-ttu-id="56b4d-107">Du behöver administratören privilegier tooprovision och ansluta tooa virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-107">You need administrator privileges tooprovision and connect tooa virtual device.</span></span> <span data-ttu-id="56b4d-108">hello etablering och inledande installationen kan ta ungefär 10 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="56b4d-108">hello provisioning and initial setup can take around 10 minutes toocomplete.</span></span>

## <a name="provisioning-prerequisites"></a><span data-ttu-id="56b4d-109">Etablering av förutsättningar</span><span class="sxs-lookup"><span data-stu-id="56b4d-109">Provisioning prerequisites</span></span>
<span data-ttu-id="56b4d-110">Hej krav tooprovision en virtuell enhet på ett värdsystem som kör VMware ESXi 5.5 och ovan, är följande.</span><span class="sxs-lookup"><span data-stu-id="56b4d-110">hello prerequisites tooprovision a virtual device on a host system running VMware ESXi 5.5 and above, are as follows.</span></span>

### <a name="for-hello-storsimple-device-manager-service"></a><span data-ttu-id="56b4d-111">För hello StorSimple enheten Manager-tjänsten</span><span class="sxs-lookup"><span data-stu-id="56b4d-111">For hello StorSimple Device Manager service</span></span>
<span data-ttu-id="56b4d-112">Innan du börjar bör du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="56b4d-112">Before you begin, make sure that:</span></span>

* <span data-ttu-id="56b4d-113">Du har slutfört alla steg i hello i [Förbered hello portal för virtuell StorSimple-matrisen](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="56b4d-113">You have completed all hello steps in [Prepare hello portal for StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>
* <span data-ttu-id="56b4d-114">Du har hämtat hello virtuella enheten avbildning för VMware från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="56b4d-114">You have downloaded hello virtual device image for VMware from hello Azure portal.</span></span> <span data-ttu-id="56b4d-115">Mer information finns **steg3: hämta hello virtuella enheten avbildningen** av [Förbered hello portal för virtuell StorSimple-matris guiden](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="56b4d-115">For more information, see **Step 3: Download hello virtual device image** of [Prepare hello portal for StorSimple Virtual Array guide](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>

### <a name="for-hello-storsimple-virtual-device"></a><span data-ttu-id="56b4d-116">För hello virtuell StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="56b4d-116">For hello StorSimple virtual device</span></span>
<span data-ttu-id="56b4d-117">Kontrollera följande innan du distribuerar en virtuell enhet:</span><span class="sxs-lookup"><span data-stu-id="56b4d-117">Before you deploy a virtual device, make sure that:</span></span>

* <span data-ttu-id="56b4d-118">Du har åtkomst tooa värdsystem som kör Hyper-V (2008 R2 eller senare) som kan vara används tooa etablera en enhet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-118">You have access tooa host system running Hyper-V (2008 R2 or later) that can be used tooa provision a device.</span></span>
* <span data-ttu-id="56b4d-119">hello värdsystem är kan toodedicate hello följande resurser tooprovision din virtuella enhet:</span><span class="sxs-lookup"><span data-stu-id="56b4d-119">hello host system is able toodedicate hello following resources tooprovision your virtual device:</span></span>

  * <span data-ttu-id="56b4d-120">Minst 4 kärnor.</span><span class="sxs-lookup"><span data-stu-id="56b4d-120">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="56b4d-121">Minst 8 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="56b4d-121">At least 8 GB of RAM.</span></span> <span data-ttu-id="56b4d-122">Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="56b4d-122">If you plan tooconfigure hello virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="56b4d-123">Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="56b4d-123">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
  * <span data-ttu-id="56b4d-124">Ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="56b4d-124">One network interface.</span></span>
  * <span data-ttu-id="56b4d-125">En virtuell disk 500 GB efter systemdata.</span><span class="sxs-lookup"><span data-stu-id="56b4d-125">A 500 GB virtual disk for system data.</span></span>

### <a name="for-hello-network-in-datacenter"></a><span data-ttu-id="56b4d-126">För hello nätverket i datacentret</span><span class="sxs-lookup"><span data-stu-id="56b4d-126">For hello network in datacenter</span></span>
<span data-ttu-id="56b4d-127">Innan du börjar bör du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="56b4d-127">Before you begin, make sure that:</span></span>

* <span data-ttu-id="56b4d-128">Du har granskat hello nätverk krav toodeploy en virtuell StorSimple-enhet och konfigurerade hello datacenternätverket enligt hello krav.</span><span class="sxs-lookup"><span data-stu-id="56b4d-128">You have reviewed hello networking requirements toodeploy a StorSimple virtual device and configured hello datacenter network as per hello requirements.</span></span> 

## <a name="step-by-step-provisioning"></a><span data-ttu-id="56b4d-129">Steg för steg-etablering</span><span class="sxs-lookup"><span data-stu-id="56b4d-129">Step-by-step provisioning</span></span>
<span data-ttu-id="56b4d-130">tooprovision och ansluta tooa virtuell enhet, måste du tooperform hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="56b4d-130">tooprovision and connect tooa virtual device, you need tooperform hello following steps:</span></span>

1. <span data-ttu-id="56b4d-131">Kontrollera att hello värdsystem har tillräckliga resurser toomeet hello minsta virtuella enhetskraven.</span><span class="sxs-lookup"><span data-stu-id="56b4d-131">Ensure that hello host system has sufficient resources toomeet hello minimum virtual device requirements.</span></span>
2. <span data-ttu-id="56b4d-132">Etablera en virtuell enhet i din hypervisor-program.</span><span class="sxs-lookup"><span data-stu-id="56b4d-132">Provision a virtual device in your hypervisor.</span></span>
3. <span data-ttu-id="56b4d-133">Starta hello virtuell enhet och hämta hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="56b4d-133">Start hello virtual device and get hello IP address.</span></span>

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a><span data-ttu-id="56b4d-134">Steg 1: Kontrollera värdsystem uppfyller minsta virtuell enhet</span><span class="sxs-lookup"><span data-stu-id="56b4d-134">Step 1: Ensure host system meets minimum virtual device requirements</span></span>
<span data-ttu-id="56b4d-135">toocreate en virtuell enhet behöver du:</span><span class="sxs-lookup"><span data-stu-id="56b4d-135">toocreate a virtual device, you will need:</span></span>

* <span data-ttu-id="56b4d-136">Åtkomst tooa värdsystem som kör VMware ESXi Server 5.5 och senare.</span><span class="sxs-lookup"><span data-stu-id="56b4d-136">Access tooa host system running VMware ESXi Server 5.5 and above.</span></span>
* <span data-ttu-id="56b4d-137">VMware vSphere-klienten på dina system toomanage hello ESXi-värd.</span><span class="sxs-lookup"><span data-stu-id="56b4d-137">VMware vSphere client on your system toomanage hello ESXi host.</span></span>

  * <span data-ttu-id="56b4d-138">Minst 4 kärnor.</span><span class="sxs-lookup"><span data-stu-id="56b4d-138">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="56b4d-139">Minst 8 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="56b4d-139">At least 8 GB of RAM.</span></span> <span data-ttu-id="56b4d-140">Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="56b4d-140">If you plan tooconfigure hello virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="56b4d-141">Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="56b4d-141">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
  * <span data-ttu-id="56b4d-142">Ett nätverksgränssnitt är anslutna till toohello kan routning trafik tooInternet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-142">One network interface connected toohello network capable of routing traffic tooInternet.</span></span> <span data-ttu-id="56b4d-143">hello Internet minimibandbredd måste vara 5 Mbit/s tooallow för optimala hello enheten att fungera.</span><span class="sxs-lookup"><span data-stu-id="56b4d-143">hello minimum Internet bandwidth should be 5 Mbps tooallow for optimal working of hello device.</span></span>
  * <span data-ttu-id="56b4d-144">En virtuell disk 500 GB för data.</span><span class="sxs-lookup"><span data-stu-id="56b4d-144">A 500 GB virtual disk for data.</span></span>

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a><span data-ttu-id="56b4d-145">Steg 2: Etablera en virtuell enhet i hypervisor-program</span><span class="sxs-lookup"><span data-stu-id="56b4d-145">Step 2: Provision a virtual device in hypervisor</span></span>
<span data-ttu-id="56b4d-146">Utför följande steg tooprovision virtuella enheter i din hypervisor-programmet hello.</span><span class="sxs-lookup"><span data-stu-id="56b4d-146">Perform hello following steps tooprovision a virtual device in your hypervisor.</span></span>

1. <span data-ttu-id="56b4d-147">Kopiera hello virtuella enheten på datorn.</span><span class="sxs-lookup"><span data-stu-id="56b4d-147">Copy hello virtual device image on your system.</span></span> <span data-ttu-id="56b4d-148">Du har hämtat den här virtuella avbildningen via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="56b4d-148">You downloaded this virtual image through hello Azure portal.</span></span>

   1. <span data-ttu-id="56b4d-149">Se till att du har hämtat hello senaste avbildningsfilen.</span><span class="sxs-lookup"><span data-stu-id="56b4d-149">Ensure that you have downloaded hello latest image file.</span></span> <span data-ttu-id="56b4d-150">Om du har hämtat hello avbildningen tidigare hämta igen tooensure hello senaste bild.</span><span class="sxs-lookup"><span data-stu-id="56b4d-150">If you downloaded hello image earlier, download it again tooensure you have hello latest image.</span></span> <span data-ttu-id="56b4d-151">hello senaste avbildningen har två filer (i stället för en).</span><span class="sxs-lookup"><span data-stu-id="56b4d-151">hello latest image has two files (instead of one).</span></span>
   2. <span data-ttu-id="56b4d-152">Anteckna hello plats dit du kopierade hello bilden som du använder den här avbildningen senare i hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="56b4d-152">Make a note of hello location where you copied hello image as you are using this image later in hello procedure.</span></span>

2. <span data-ttu-id="56b4d-153">Logga in toohello ESXi-servern använder hello vSphere-klienten.</span><span class="sxs-lookup"><span data-stu-id="56b4d-153">Log in toohello ESXi server using hello vSphere client.</span></span> <span data-ttu-id="56b4d-154">Du måste toohave administratör privilegier toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="56b4d-154">You need toohave administrator privileges toocreate a virtual machine.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. <span data-ttu-id="56b4d-155">Välj hello ESXi-servern i hello vSphere-klient under hello inventering i hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="56b4d-155">In hello vSphere client, in hello inventory section in hello left pane, select hello ESXi Server.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. <span data-ttu-id="56b4d-156">Överför hello VMDK toohello ESXi-servern.</span><span class="sxs-lookup"><span data-stu-id="56b4d-156">Upload hello VMDK toohello ESXi server.</span></span> <span data-ttu-id="56b4d-157">Navigera toohello **Configuration** fliken i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="56b4d-157">Navigate toohello **Configuration** tab in hello right pane.</span></span> <span data-ttu-id="56b4d-158">Under **maskinvara**väljer **lagring**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-158">Under **Hardware**, select **Storage**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. <span data-ttu-id="56b4d-159">I hello Högerklicka rutan under **Datastores**, Välj hello datalagret där du vill att tooupload hello VMDK.</span><span class="sxs-lookup"><span data-stu-id="56b4d-159">In hello right pane, under **Datastores**, select hello datastore where you want tooupload hello VMDK.</span></span> <span data-ttu-id="56b4d-160">hello datalagret måste ha tillräckligt med ledigt utrymme för hello OS och datadiskar.</span><span class="sxs-lookup"><span data-stu-id="56b4d-160">hello datastore must have enough free space for hello OS and data disks.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. <span data-ttu-id="56b4d-161">Högerklicka och välj **Bläddra Datastore**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-161">Right-click and select **Browse Datastore**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. <span data-ttu-id="56b4d-162">En **Datastore webbläsare** visas.</span><span class="sxs-lookup"><span data-stu-id="56b4d-162">A **Datastore Browser** window appears.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. <span data-ttu-id="56b4d-163">Klicka på i verktygsfältet för hello ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) ikonen toocreate en ny mapp.</span><span class="sxs-lookup"><span data-stu-id="56b4d-163">In hello tool bar, click ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) icon toocreate a new folder.</span></span> <span data-ttu-id="56b4d-164">Ange hello mappnamn och anteckna den.</span><span class="sxs-lookup"><span data-stu-id="56b4d-164">Specify hello folder name and make a note of it.</span></span> <span data-ttu-id="56b4d-165">Du använder den här mappnamn senare när du skapar en virtuell dator (rekommenderas för bästa praxis).</span><span class="sxs-lookup"><span data-stu-id="56b4d-165">You will use this folder name later when creating a virtual machine (recommended best practice).</span></span> <span data-ttu-id="56b4d-166">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-166">Click **OK**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. <span data-ttu-id="56b4d-167">hello nya mappen visas i hello till vänster i hello **Datastore webbläsare**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-167">hello new folder appears in hello left pane of hello **Datastore Browser**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. <span data-ttu-id="56b4d-168">Klicka på ikonen för hello överför ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) och välj **överför filen**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-168">Click hello Upload icon ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) and select **Upload File**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. <span data-ttu-id="56b4d-169">Bläddra och toohello VMDK-filer som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="56b4d-169">Browse and point toohello VMDK files that you downloaded.</span></span> <span data-ttu-id="56b4d-170">Det finns två filer.</span><span class="sxs-lookup"><span data-stu-id="56b4d-170">There are two files.</span></span> <span data-ttu-id="56b4d-171">Välj en fil tooupload.</span><span class="sxs-lookup"><span data-stu-id="56b4d-171">Select a file tooupload.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. <span data-ttu-id="56b4d-172">Klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-172">Click **Open**.</span></span> <span data-ttu-id="56b4d-173">hello överföringen av hello VMDK-filen toohello angetts datastore startar.</span><span class="sxs-lookup"><span data-stu-id="56b4d-173">hello upload of hello VMDK file toohello specified datastore starts.</span></span> <span data-ttu-id="56b4d-174">Det kan ta flera minuter för hello filen tooupload.</span><span class="sxs-lookup"><span data-stu-id="56b4d-174">It may take several minutes for hello file tooupload.</span></span>
13. <span data-ttu-id="56b4d-175">När hello överföringen är klar, visas hello-filen i hello datalagret i hello-mappen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="56b4d-175">After hello upload is complete, you see hello file in hello datastore in hello folder you created.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    <span data-ttu-id="56b4d-176">Ladda upp hello andra VMDK-filen toohello samma datalager.</span><span class="sxs-lookup"><span data-stu-id="56b4d-176">Now upload hello second VMDK file toohello same datastore.</span></span>
14. <span data-ttu-id="56b4d-177">Returnera toohello vSphere klienten fönster.</span><span class="sxs-lookup"><span data-stu-id="56b4d-177">Return toohello vSphere client window.</span></span> <span data-ttu-id="56b4d-178">Med ESXi-servern är markerad, högerklicka och välj **ny virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-178">With ESXi server selected, right-click and select **New Virtual Machine**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. <span data-ttu-id="56b4d-179">En **Skapa ny virtuell dator** fönster visas.</span><span class="sxs-lookup"><span data-stu-id="56b4d-179">A **Create New Virtual Machine** window will appear.</span></span> <span data-ttu-id="56b4d-180">På hello **Configuration** sidan, Välj hello **anpassad** alternativet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-180">On hello **Configuration** page, select hello **Custom** option.</span></span> <span data-ttu-id="56b4d-181">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-181">Click **Next**.</span></span>
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. <span data-ttu-id="56b4d-182">På hello **namn och plats** anger hello namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="56b4d-182">On hello **Name and Location** page, specify hello name of your virtual machine.</span></span> <span data-ttu-id="56b4d-183">Det här namnet ska matcha hello (rekommenderas) det angivna mappnamnet tidigare i steg 8.</span><span class="sxs-lookup"><span data-stu-id="56b4d-183">This name should match hello folder name (recommended best practice) you specified earlier in Step 8.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. <span data-ttu-id="56b4d-184">På hello **lagring** väljer du ett datalager som du vill toouse tooprovision den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="56b4d-184">On hello **Storage** page, select a datastore you want toouse tooprovision your VM.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. <span data-ttu-id="56b4d-185">På hello **virtuella Version** väljer **virtuella Version: 8**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-185">On hello **Virtual Machine Version** page, select **Virtual Machine Version: 8**.</span></span> <span data-ttu-id="56b4d-186">Versioner 8 too11 stöds.</span><span class="sxs-lookup"><span data-stu-id="56b4d-186">Versions 8 too11 are all supported.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. <span data-ttu-id="56b4d-187">På hello **gästoperativsystemet** sidan, Välj hello **gästoperativsystemet** som **Windows**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-187">On hello **Guest Operating System** page, select hello **Guest Operating System** as **Windows**.</span></span> <span data-ttu-id="56b4d-188">För **Version**, hello listrutan, Välj **Microsoft Windows Server 2012 (64-bitars)**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-188">For **Version**, from hello dropdown list, select **Microsoft Windows Server 2012 (64-bit)**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. <span data-ttu-id="56b4d-189">På hello **processorer** sidan, justera hello **antalet virtuella uttag** och **antalet kärnor per virtuell socket** så som hello **Totalt antal kärnor**4 (eller mer).</span><span class="sxs-lookup"><span data-stu-id="56b4d-189">On hello **CPUs** page, adjust hello **Number of virtual sockets** and **Number of cores per virtual socket** so that hello **Total number of cores** is 4 (or more).</span></span> <span data-ttu-id="56b4d-190">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-190">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. <span data-ttu-id="56b4d-191">På hello **minne** anger 8 GB (eller mer) RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="56b4d-191">On hello **Memory** page, specify 8 GB (or more) of RAM.</span></span> <span data-ttu-id="56b4d-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-192">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. <span data-ttu-id="56b4d-193">På hello **nätverk** anger hello antalet hello nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="56b4d-193">On hello **Network** page, specify hello number of hello network interfaces.</span></span> <span data-ttu-id="56b4d-194">hello Minimikravet är ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="56b4d-194">hello minimum requirement is one network interface.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. <span data-ttu-id="56b4d-195">På hello **SCSI-styrenhet** godkänner hello standard **LSI Logic SAS-styrenhet**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-195">On hello **SCSI Controller** page, accept hello default **LSI Logic SAS controller**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. <span data-ttu-id="56b4d-196">På hello **Välj en Disk** väljer **använder en befintlig virtuell disk**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-196">On hello **Select a Disk** page, choose **Use an existing virtual disk**.</span></span> <span data-ttu-id="56b4d-197">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-197">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. <span data-ttu-id="56b4d-198">På hello **Välj befintlig Disk** sidan under **Disk filsökväg**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-198">On hello **Select Existing Disk** page, under **Disk File Path**, click **Browse**.</span></span> <span data-ttu-id="56b4d-199">Då öppnas en **Bläddra Datastores** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="56b4d-199">This opens a **Browse Datastores** dialog.</span></span> <span data-ttu-id="56b4d-200">Navigera toohello plats där du har överfört hello VMDK.</span><span class="sxs-lookup"><span data-stu-id="56b4d-200">Navigate toohello location where you uploaded hello VMDK.</span></span> <span data-ttu-id="56b4d-201">Du kan nu se en enstaka fil i hello datalagret som hello två filer som du ursprungligen har överfört har slagits samman.</span><span class="sxs-lookup"><span data-stu-id="56b4d-201">You now see only one file in hello datastore as hello two files that you initially uploaded have been merged.</span></span> <span data-ttu-id="56b4d-202">Välj hello-filen och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-202">Select hello file and click **OK**.</span></span> <span data-ttu-id="56b4d-203">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-203">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. <span data-ttu-id="56b4d-204">På hello **avancerade alternativ** accepterar hello standard och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-204">On hello **Advanced Options** page, accept hello default and click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. <span data-ttu-id="56b4d-205">På hello **klar tooComplete** granskar alla hello inställningar hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="56b4d-205">On hello **Ready tooComplete** page, review all hello settings associated with hello new virtual machine.</span></span> <span data-ttu-id="56b4d-206">Kontrollera **redigera hello inställningarna för virtuella datorer före slutförande**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-206">Check **Edit hello virtual machine settings before completion**.</span></span> <span data-ttu-id="56b4d-207">Klicka på **fortsätta**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-207">Click **Continue**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. <span data-ttu-id="56b4d-208">På hello **egenskaper för virtuella datorer** i hello sidan **maskinvara** , letar du reda hello maskinvara.</span><span class="sxs-lookup"><span data-stu-id="56b4d-208">On hello **Virtual Machines Properties** page, in hello **Hardware** tab, locate hello device hardware.</span></span> <span data-ttu-id="56b4d-209">Välj **ny hårddisk**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-209">Select **New Hard Disk**.</span></span> <span data-ttu-id="56b4d-210">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-210">Click **Add**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. <span data-ttu-id="56b4d-211">Du ser en **Lägg till maskinvara** fönster.</span><span class="sxs-lookup"><span data-stu-id="56b4d-211">You see a **Add Hardware** window.</span></span> <span data-ttu-id="56b4d-212">På hello **enhetstyp** sidan under **Välj hello typ av enhet du vill tooadd**väljer **hårddisk**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-212">On hello **Device Type** page, under **Choose hello type of device you wish tooadd**, select **Hard Disk**, and click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. <span data-ttu-id="56b4d-213">På hello **Välj en Disk** väljer **skapar en ny virtuell disk**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-213">On hello **Select a Disk** page, choose **Create a new virtual disk**.</span></span> <span data-ttu-id="56b4d-214">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-214">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. <span data-ttu-id="56b4d-215">På hello **skapar du en Disk** ändrar hello **diskstorleken** too500 GB (eller mer).</span><span class="sxs-lookup"><span data-stu-id="56b4d-215">On hello **Create a Disk** page, change hello **Disk Size** too500 GB (or more).</span></span> <span data-ttu-id="56b4d-216">500 GB är minimikravet för hello, kan du alltid etablera en större disk.</span><span class="sxs-lookup"><span data-stu-id="56b4d-216">While 500 GB is hello minimum requirement, you can always provision a larger disk.</span></span> <span data-ttu-id="56b4d-217">Observera att du kan öka eller minska hello-disk som etablerats en gång.</span><span class="sxs-lookup"><span data-stu-id="56b4d-217">Note that you cannot expand or shrink hello disk once provisioned.</span></span> <span data-ttu-id="56b4d-218">Mer information om hello storleken på disken tooprovision granska hello sizing avsnitt i hello [bästa praxis dokumentet](storsimple-ova-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="56b4d-218">For more information on hello size of disk tooprovision, review hello sizing section in hello [best practices document](storsimple-ova-best-practices.md).</span></span> <span data-ttu-id="56b4d-219">Under **Disk etablering**väljer **tunn**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-219">Under **Disk Provisioning**, select **Thin Provision**.</span></span> <span data-ttu-id="56b4d-220">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-220">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. <span data-ttu-id="56b4d-221">På hello **avancerade alternativ** godkänner hello standard.</span><span class="sxs-lookup"><span data-stu-id="56b4d-221">On hello **Advanced Options** page, accept hello default.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. <span data-ttu-id="56b4d-222">På hello **klar tooComplete** läser du igenom alternativen för hello-diskar.</span><span class="sxs-lookup"><span data-stu-id="56b4d-222">On hello **Ready tooComplete** page, review hello disk options.</span></span> <span data-ttu-id="56b4d-223">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-223">Click **Finish**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. <span data-ttu-id="56b4d-224">Gå tillbaka toohello egenskaper för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="56b4d-224">Return toohello Virtual Machine Properties page.</span></span> <span data-ttu-id="56b4d-225">En ny hårddisk läggs tooyour virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="56b4d-225">A new hard disk is added tooyour virtual machine.</span></span> <span data-ttu-id="56b4d-226">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-226">Click **Finish**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. <span data-ttu-id="56b4d-227">Med den virtuella datorn har valts i hello högra, navigera toohello **sammanfattning** fliken. Kontrollera hello inställningarna för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="56b4d-227">With your virtual machine selected in hello right pane, navigate toohello **Summary** tab. Review hello settings for your virtual machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

<span data-ttu-id="56b4d-228">Den virtuella datorn har nu etablerats.</span><span class="sxs-lookup"><span data-stu-id="56b4d-228">Your virtual machine is now provisioned.</span></span> <span data-ttu-id="56b4d-229">hello nästa steg är toopower på den här datorn och hämta hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="56b4d-229">hello next step is toopower on this machine and get hello IP address.</span></span>

## <a name="step-3-start-hello-virtual-device-and-get-hello-ip"></a><span data-ttu-id="56b4d-230">Steg 3: Starta hello virtuell enhet och få hello IP</span><span class="sxs-lookup"><span data-stu-id="56b4d-230">Step 3: Start hello virtual device and get hello IP</span></span>
<span data-ttu-id="56b4d-231">Utför följande steg toostart hello din virtuella enhet och ansluta tooit.</span><span class="sxs-lookup"><span data-stu-id="56b4d-231">Perform hello following steps toostart your virtual device and connect tooit.</span></span>

#### <a name="toostart-hello-virtual-device"></a><span data-ttu-id="56b4d-232">toostart hello virtuell enhet</span><span class="sxs-lookup"><span data-stu-id="56b4d-232">toostart hello virtual device</span></span>
1. <span data-ttu-id="56b4d-233">Starta hello virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-233">Start hello virtual device.</span></span> <span data-ttu-id="56b4d-234">Välj din enhet i hello vSphere Configuration Manager hello vänster och högerklicka på toobring in hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="56b4d-234">In hello vSphere Configuration Manager, in hello left pane, select your device and right-click toobring up hello context menu.</span></span> <span data-ttu-id="56b4d-235">Välj **Power** och välj sedan **ström på**.</span><span class="sxs-lookup"><span data-stu-id="56b4d-235">Select **Power** and then select **Power on**.</span></span> <span data-ttu-id="56b4d-236">Detta bör sätta på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="56b4d-236">This should power on your virtual machine.</span></span> <span data-ttu-id="56b4d-237">Du kan visa statusen för hello i hello nedre **senaste aktiviteter** rutan av hello vSphere-klienten.</span><span class="sxs-lookup"><span data-stu-id="56b4d-237">You can view hello status in hello bottom **Recent Tasks** pane of hello vSphere client.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. <span data-ttu-id="56b4d-238">hello installationsaktiviteter tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="56b4d-238">hello setup tasks will take a few minutes toocomplete.</span></span> <span data-ttu-id="56b4d-239">När enheten hello körs kan navigera toohello **konsolen** fliken. Skicka Ctrl + Alt + Delete toolog i toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-239">Once hello device is running, navigate toohello **Console** tab. Send Ctrl+Alt+Delete toolog in toohello device.</span></span> <span data-ttu-id="56b4d-240">Du kan också peka hello markören på hello konsolfönster och tryck på Ctrl + Alt + Insert.</span><span class="sxs-lookup"><span data-stu-id="56b4d-240">Alternatively, you can point hello cursor on hello console window and press Ctrl+Alt+Insert.</span></span> <span data-ttu-id="56b4d-241">hello standardanvändaren är *StorSimpleAdmin* och hello standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="56b4d-241">hello default user is *StorSimpleAdmin* and hello default password is *Password1*.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. <span data-ttu-id="56b4d-242">Av säkerhetsskäl upphör hello enhetens administratörslösenord vid hello första inloggningen.</span><span class="sxs-lookup"><span data-stu-id="56b4d-242">For security reasons, hello device administrator password expires at hello first logon.</span></span> <span data-ttu-id="56b4d-243">Du kan ange toochange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="56b4d-243">You are prompted toochange hello password.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. <span data-ttu-id="56b4d-244">Ange ett lösenord som innehåller minst 8 tecken.</span><span class="sxs-lookup"><span data-stu-id="56b4d-244">Enter a password that contains at least 8 characters.</span></span> <span data-ttu-id="56b4d-245">hello lösenordet måste innehålla 3 av 4 av dessa krav: versaler, gemener, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="56b4d-245">hello password must contain 3 out of 4 of these requirements: uppercase, lowercase, numeric, and special characters.</span></span> <span data-ttu-id="56b4d-246">Ange hello lösenord tooconfirm den.</span><span class="sxs-lookup"><span data-stu-id="56b4d-246">Reenter hello password tooconfirm it.</span></span> <span data-ttu-id="56b4d-247">Du meddelas hello lösenordet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="56b4d-247">You will be notified that hello password has changed.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. <span data-ttu-id="56b4d-248">När hello lösenordet har ändrats hello virtuella enheten kan starta om.</span><span class="sxs-lookup"><span data-stu-id="56b4d-248">After hello password is successfully changed, hello virtual device may reboot.</span></span> <span data-ttu-id="56b4d-249">Vänta tills hello omstart toocomplete.</span><span class="sxs-lookup"><span data-stu-id="56b4d-249">Wait for hello reboot toocomplete.</span></span> <span data-ttu-id="56b4d-250">hello Windows PowerShell-konsol för hello enhet visas tillsammans med en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="56b4d-250">hello Windows PowerShell console of hello device may be displayed along with a progress bar.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. <span data-ttu-id="56b4d-251">Steg 6 – 8 gäller endast när du startar upp i en icke-DHCP-miljö.</span><span class="sxs-lookup"><span data-stu-id="56b4d-251">Steps 6-8 only apply when booting up in a non-DHCP environment.</span></span> <span data-ttu-id="56b4d-252">Om du har en DHCP-miljö, hoppa över de här stegen och går toostep 9.</span><span class="sxs-lookup"><span data-stu-id="56b4d-252">If you are in a DHCP environment, then skip these steps and go toostep 9.</span></span> <span data-ttu-id="56b4d-253">Om du har startat upp din enhet i DHCP-miljön visas hello följande skärm.</span><span class="sxs-lookup"><span data-stu-id="56b4d-253">If you booted up your device in non-DHCP environment, you will see hello following screen.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   <span data-ttu-id="56b4d-254">Konfigurera hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="56b4d-254">Next, configure hello network.</span></span>
7. <span data-ttu-id="56b4d-255">Använd hello `Get-HcsIpAddress` kommandot toolist hello nätverksgränssnitt aktiverad på din virtuella enhet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-255">Use hello `Get-HcsIpAddress` command toolist hello network interfaces enabled on your virtual device.</span></span> <span data-ttu-id="56b4d-256">Om enheten har ett enda nätverksgränssnitt som är aktiverad, hello standardgränssnittet tilldelade toothis är `Ethernet`.</span><span class="sxs-lookup"><span data-stu-id="56b4d-256">If your device has a single network interface enabled, hello default name assigned toothis interface is `Ethernet`.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. <span data-ttu-id="56b4d-257">Använd hello `Set-HcsIpAddress` cmdlet tooconfigure hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="56b4d-257">Use hello `Set-HcsIpAddress` cmdlet tooconfigure hello network.</span></span> <span data-ttu-id="56b4d-258">Ett exempel visas nedan:</span><span class="sxs-lookup"><span data-stu-id="56b4d-258">An example is shown below:</span></span>

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. <span data-ttu-id="56b4d-259">När hello installationen är klar och hello enheten har startats in, visas hello banderolltext för enheten.</span><span class="sxs-lookup"><span data-stu-id="56b4d-259">After hello initial setup is complete and hello device has booted up, you will see hello device banner text.</span></span> <span data-ttu-id="56b4d-260">Anteckna hello IP-adress och hello-URL som visas i hello banderoll text toomanage hello enhet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-260">Make a note of hello IP address and hello URL displayed in hello banner text toomanage hello device.</span></span> <span data-ttu-id="56b4d-261">Du använder den här IP-adressen tooconnect toohello webbgränssnittet för din virtuella enhet och fullständig hello lokal installation och registrering.</span><span class="sxs-lookup"><span data-stu-id="56b4d-261">You will use this IP address tooconnect toohello web UI of your virtual device and complete hello local setup and registration.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. <span data-ttu-id="56b4d-262">(Valfritt) Utför det här steget endast om du distribuerar din enhet i hello offentliga moln.</span><span class="sxs-lookup"><span data-stu-id="56b4d-262">(Optional) Perform this step only if you are deploying your device in hello Government Cloud.</span></span> <span data-ttu-id="56b4d-263">Du kan nu hello USA FIPS Federal Information Processing Standard ()-läge på enheten.</span><span class="sxs-lookup"><span data-stu-id="56b4d-263">You will now enable hello United States Federal Information Processing Standard (FIPS) mode on your device.</span></span> <span data-ttu-id="56b4d-264">hello FIPS 140-standarden definierar kryptografiska algoritmer som godkänts för användning av oss federala myndigheter datorsystem för hello skyddet av känsliga data.</span><span class="sxs-lookup"><span data-stu-id="56b4d-264">hello FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for hello protection of sensitive data.</span></span>

    1. <span data-ttu-id="56b4d-265">tooenable hello FIPS-läge, kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="56b4d-265">tooenable hello FIPS mode, run hello following cmdlet:</span></span>

        `Enable-HcsFIPSMode`
    2. <span data-ttu-id="56b4d-266">Starta om enheten när du har aktiverat hello FIPS-läge så att hello kryptografiska verifieringar börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="56b4d-266">Reboot your device after you have enabled hello FIPS mode so that hello cryptographic validations take effect.</span></span>

       > [!NOTE]
       > <span data-ttu-id="56b4d-267">Du kan aktivera eller inaktivera FIPS-läge på enheten.</span><span class="sxs-lookup"><span data-stu-id="56b4d-267">You can either enable or disable FIPS mode on your device.</span></span> <span data-ttu-id="56b4d-268">Alternerande hello enhet mellan FIPS och icke-FIPS-läge stöds inte.</span><span class="sxs-lookup"><span data-stu-id="56b4d-268">Alternating hello device between FIPS and non-FIPS mode is not supported.</span></span>
       >
       >

<span data-ttu-id="56b4d-269">Om enheten inte uppfyller lägsta konfigurationskrav som hello, visas ett fel i hello banderolltext (se nedan).</span><span class="sxs-lookup"><span data-stu-id="56b4d-269">If your device does not meet hello minimum configuration requirements, you will see an error in hello banner text (shown below).</span></span> <span data-ttu-id="56b4d-270">Du behöver toomodify hello enhetskonfigurationen så att det finns tillräckliga resurser toomeet hello minimikraven.</span><span class="sxs-lookup"><span data-stu-id="56b4d-270">You will need toomodify hello device configuration so that it has adequate resources toomeet hello minimum requirements.</span></span> <span data-ttu-id="56b4d-271">Du kan sedan starta om och Anslut toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="56b4d-271">You can then restart and connect toohello device.</span></span> <span data-ttu-id="56b4d-272">Se toohello lägsta konfigurationskrav i [steg 1: Kontrollera att hello värdsystem uppfyller minsta virtuella enheten](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).</span><span class="sxs-lookup"><span data-stu-id="56b4d-272">Refer toohello minimum configuration requirements in [Step 1: Ensure that hello host system meets minimum virtual device requirements](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).</span></span>

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

<span data-ttu-id="56b4d-273">Om du står inför ett fel under hello startkonfiguration med hello lokala webbgränssnittet Se toohello följande arbetsflöden:</span><span class="sxs-lookup"><span data-stu-id="56b4d-273">If you face any other error during hello initial configuration using hello local web UI, refer toohello following workflows:</span></span>

* <span data-ttu-id="56b4d-274">Kör diagnostiktest för[felsöka installationen av UI](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span><span class="sxs-lookup"><span data-stu-id="56b4d-274">Run diagnostic tests too[troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span></span>
* <span data-ttu-id="56b4d-275">[Generera loggen paketet och visa loggfiler](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="56b4d-275">[Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56b4d-276">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56b4d-276">Next steps</span></span>
* [<span data-ttu-id="56b4d-277">Konfigurera din virtuella StorSimple-matris som en filserver</span><span class="sxs-lookup"><span data-stu-id="56b4d-277">Set up your StorSimple Virtual Array as a file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)
* [<span data-ttu-id="56b4d-278">Konfigurera din virtuella StorSimple-matris som en iSCSI-server</span><span class="sxs-lookup"><span data-stu-id="56b4d-278">Set up your StorSimple Virtual Array as an iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md)
