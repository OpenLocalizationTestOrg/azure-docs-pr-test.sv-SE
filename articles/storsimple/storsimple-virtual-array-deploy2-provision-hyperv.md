---
title: "aaaProvision virtuella StorSimple-matris på Hyper-V | Microsoft Docs"
description: "Självstudierna andra i virtuella StorSimple-matris distribution innebär att etablera en virtuell Hyper-V-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f47d642f740827ae1440b819e07067c6a183527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a><span data-ttu-id="8edbf-103">Distribuera StorSimple virtuell matris - etablera i Hyper-V</span><span class="sxs-lookup"><span data-stu-id="8edbf-103">Deploy StorSimple Virtual Array - Provision in Hyper-V</span></span>
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a><span data-ttu-id="8edbf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8edbf-104">Overview</span></span>
<span data-ttu-id="8edbf-105">Den här självstudiekursen beskrivs hur tooprovision en StorSimple virtuell matrisen på ett värdsystem som kör Hyper-V på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="8edbf-105">This tutorial describes how tooprovision a StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <span data-ttu-id="8edbf-106">Den här artikeln gäller toohello distribution av virtuella StorSimple-matriser i Azure-portalen och Microsoft Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="8edbf-106">This article applies toohello deployment of StorSimple Virtual Arrays in Azure portal and Microsoft Azure Government Cloud.</span></span>

<span data-ttu-id="8edbf-107">Du måste administratören privilegier tooprovision och konfigurerar en virtuell matris.</span><span class="sxs-lookup"><span data-stu-id="8edbf-107">You need administrator privileges tooprovision and configure a virtual array.</span></span> <span data-ttu-id="8edbf-108">hello etablering och inledande installationen kan ta ungefär 10 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8edbf-108">hello provisioning and initial setup can take around 10 minutes toocomplete.</span></span>

## <a name="provisioning-prerequisites"></a><span data-ttu-id="8edbf-109">Etablering av förutsättningar</span><span class="sxs-lookup"><span data-stu-id="8edbf-109">Provisioning prerequisites</span></span>
<span data-ttu-id="8edbf-110">Här hittar du hello krav tooprovision virtuella matrisen på ett värdsystem som kör Hyper-V på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="8edbf-110">Here you will find hello prerequisites tooprovision a virtual array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span>

### <a name="for-hello-storsimple-device-manager-service"></a><span data-ttu-id="8edbf-111">För hello StorSimple enheten Manager-tjänsten</span><span class="sxs-lookup"><span data-stu-id="8edbf-111">For hello StorSimple Device Manager service</span></span>
<span data-ttu-id="8edbf-112">Innan du börjar bör du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="8edbf-112">Before you begin, make sure that:</span></span>

* <span data-ttu-id="8edbf-113">Du har slutfört alla steg i hello i [Förbered hello portal för virtuell StorSimple-matrisen](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="8edbf-113">You have completed all hello steps in [Prepare hello portal for StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>
* <span data-ttu-id="8edbf-114">Du har hämtat hello virtuella matris avbildning för Hyper-V från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8edbf-114">You have downloaded hello virtual array image for Hyper-V from hello Azure portal.</span></span> <span data-ttu-id="8edbf-115">Mer information finns **steg3: hämta hello virtuella matris avbildningen** av [Förbered hello portal för virtuell StorSimple-matris guiden](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="8edbf-115">For more information, see **Step 3: Download hello virtual array image** of [Prepare hello portal for StorSimple Virtual Array guide](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="8edbf-116">hello-program som körs på hello virtuella StorSimple-matrisen får endast användas med hello StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8edbf-116">hello software running on hello StorSimple Virtual Array may only be used with hello StorSimple Device Manager service.</span></span>
  >
  >

### <a name="for-hello-storsimple-virtual-array"></a><span data-ttu-id="8edbf-117">För hello virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="8edbf-117">For hello StorSimple Virtual Array</span></span>
<span data-ttu-id="8edbf-118">Innan du distribuerar en virtuell matris, kontrollerar du att:</span><span class="sxs-lookup"><span data-stu-id="8edbf-118">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="8edbf-119">Du har åtkomst tooa värdsystem som kör Hyper-V på Windows Server 2008 R2 eller senare att kan vara används tooa etablera en enhet.</span><span class="sxs-lookup"><span data-stu-id="8edbf-119">You have access tooa host system running Hyper-V on Windows Server 2008 R2 or later that can be used tooa provision a device.</span></span>
* <span data-ttu-id="8edbf-120">hello värdsystem är kan toodedicate hello följande resurser tooprovision virtuella matrisen:</span><span class="sxs-lookup"><span data-stu-id="8edbf-120">hello host system is able toodedicate hello following resources tooprovision your virtual array:</span></span>

  * <span data-ttu-id="8edbf-121">Minst 4 kärnor.</span><span class="sxs-lookup"><span data-stu-id="8edbf-121">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="8edbf-122">Minst 8 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="8edbf-122">At least 8 GB of RAM.</span></span> <span data-ttu-id="8edbf-123">Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="8edbf-123">If you plan tooconfigure hello virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="8edbf-124">Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="8edbf-124">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
  * <span data-ttu-id="8edbf-125">Ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="8edbf-125">One network interface.</span></span>
  * <span data-ttu-id="8edbf-126">En virtuell disk 500 GB för data.</span><span class="sxs-lookup"><span data-stu-id="8edbf-126">A 500 GB virtual disk for data.</span></span>

### <a name="for-hello-network-in-hello-datacenter"></a><span data-ttu-id="8edbf-127">För hello nätverket i hello datacenter</span><span class="sxs-lookup"><span data-stu-id="8edbf-127">For hello network in hello datacenter</span></span>
<span data-ttu-id="8edbf-128">Innan du kan granska hello nätverk krav toodeploy en virtuell StorSimple-matris och konfigurera hello datacenternätverket på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="8edbf-128">Before you begin, review hello networking requirements toodeploy a StorSimple Virtual Array and configure hello datacenter network appropriately.</span></span> <span data-ttu-id="8edbf-129">Mer information finns i [StorSimple virtuell matris nätverkskrav](storsimple-ova-system-requirements.md#networking-requirements).</span><span class="sxs-lookup"><span data-stu-id="8edbf-129">For more information, see [StorSimple Virtual Array networking requirements](storsimple-ova-system-requirements.md#networking-requirements).</span></span>

## <a name="step-by-step-provisioning"></a><span data-ttu-id="8edbf-130">Steg för steg-etablering</span><span class="sxs-lookup"><span data-stu-id="8edbf-130">Step-by-step provisioning</span></span>
<span data-ttu-id="8edbf-131">tooprovision och ansluta tooa virtuella matrisen måste du tooperform hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8edbf-131">tooprovision and connect tooa virtual array, you need tooperform hello following steps:</span></span>

1. <span data-ttu-id="8edbf-132">Kontrollera att hello värdsystem har tillräckliga resurser toomeet hello minsta virtuella matris krav.</span><span class="sxs-lookup"><span data-stu-id="8edbf-132">Ensure that hello host system has sufficient resources toomeet hello minimum virtual array requirements.</span></span>
2. <span data-ttu-id="8edbf-133">Etablera en virtuell matris i din hypervisor-program.</span><span class="sxs-lookup"><span data-stu-id="8edbf-133">Provision a virtual array in your hypervisor.</span></span>
3. <span data-ttu-id="8edbf-134">Starta virtuella hello-matris och hämta hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8edbf-134">Start hello virtual array and get hello IP address.</span></span>

<span data-ttu-id="8edbf-135">Var och en av de här stegen beskrivs hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8edbf-135">Each of these steps is explained in hello following sections.</span></span>

## <a name="step-1-ensure-that-hello-host-system-meets-minimum-virtual-array-requirements"></a><span data-ttu-id="8edbf-136">Steg 1: Kontrollera att hello värdsystem uppfyller minsta virtuella matris</span><span class="sxs-lookup"><span data-stu-id="8edbf-136">Step 1: Ensure that hello host system meets minimum virtual array requirements</span></span>
<span data-ttu-id="8edbf-137">toocreate virtuella matris, behöver du:</span><span class="sxs-lookup"><span data-stu-id="8edbf-137">toocreate a virtual array, you need:</span></span>

* <span data-ttu-id="8edbf-138">hello Hyper-V-rollen är installerad på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="8edbf-138">hello Hyper-V role installed on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2 SP1.</span></span>
* <span data-ttu-id="8edbf-139">Microsoft Hyper-V Manager på en Microsoft Windows-klient ansluten toohello värden.</span><span class="sxs-lookup"><span data-stu-id="8edbf-139">Microsoft Hyper-V Manager on a Microsoft Windows client connected toohello host.</span></span>

<span data-ttu-id="8edbf-140">Se till att hello underliggande maskinvara (värdsystem) som du skapar virtuella hello-matris kan toodedicate hello följande resurser tooyour virtuella matris:</span><span class="sxs-lookup"><span data-stu-id="8edbf-140">Make sure that hello underlying hardware (host system) on which you are creating hello virtual array is able toodedicate hello following resources tooyour virtual array:</span></span>

* <span data-ttu-id="8edbf-141">Minst 4 kärnor.</span><span class="sxs-lookup"><span data-stu-id="8edbf-141">A minimum of 4 cores.</span></span>
* <span data-ttu-id="8edbf-142">Minst 8 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="8edbf-142">At least 8 GB of RAM.</span></span> <span data-ttu-id="8edbf-143">Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB mindre än 2 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="8edbf-143">If you plan tooconfigure hello virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="8edbf-144">Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="8edbf-144">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
* <span data-ttu-id="8edbf-145">Ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="8edbf-145">One network interface.</span></span>
* <span data-ttu-id="8edbf-146">En virtuell disk 500 GB efter systemdata.</span><span class="sxs-lookup"><span data-stu-id="8edbf-146">A 500 GB virtual disk for system data.</span></span>

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a><span data-ttu-id="8edbf-147">Steg 2: Etablera en virtuell matris i hypervisor-program</span><span class="sxs-lookup"><span data-stu-id="8edbf-147">Step 2: Provision a virtual array in hypervisor</span></span>
<span data-ttu-id="8edbf-148">Utför följande steg tooprovision en enhet i din hypervisor-programmet hello.</span><span class="sxs-lookup"><span data-stu-id="8edbf-148">Perform hello following steps tooprovision a device in your hypervisor.</span></span>

#### <a name="tooprovision-a-virtual-array"></a><span data-ttu-id="8edbf-149">tooprovision virtuella matris</span><span class="sxs-lookup"><span data-stu-id="8edbf-149">tooprovision a virtual array</span></span>
1. <span data-ttu-id="8edbf-150">Kopiera hello virtuella matris avbildningen tooa lokal enhet på Windows Server-värd.</span><span class="sxs-lookup"><span data-stu-id="8edbf-150">On your Windows Server host, copy hello virtual array image tooa local drive.</span></span> <span data-ttu-id="8edbf-151">Du har hämtat den här avbildningen (VHD eller VHDX) via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8edbf-151">You downloaded this image (VHD or VHDX) through hello Azure portal.</span></span> <span data-ttu-id="8edbf-152">Anteckna hello plats dit du kopierade hello bilden som du använder den här avbildningen senare i hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="8edbf-152">Make a note of hello location where you copied hello image as you are using this image later in hello procedure.</span></span>
2. <span data-ttu-id="8edbf-153">Öppna **Serverhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-153">Open **Server Manager**.</span></span> <span data-ttu-id="8edbf-154">I hello längst upp till höger klickar du på **verktyg** och välj **Hyper-V Manager**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-154">In hello top right corner, click **Tools** and select **Hyper-V Manager**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   <span data-ttu-id="8edbf-155">Om du kör Windows Server 2008 R2, öppna hello Hyper-V Manager.</span><span class="sxs-lookup"><span data-stu-id="8edbf-155">If you are running Windows Server 2008 R2, open hello Hyper-V Manager.</span></span> <span data-ttu-id="8edbf-156">I Serverhanteraren klickar du på **roller > Hyper-V > Hyper-V Manager**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-156">In Server Manager, click **Roles > Hyper-V > Hyper-V Manager**.</span></span>
3. <span data-ttu-id="8edbf-157">I **Hyper-V Manager**hello scope i fönstret, högerklicka på ditt system nod tooopen hello snabbmenyn, och klicka sedan på **ny** > **virtuella**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-157">In **Hyper-V Manager**, in hello scope pane, right-click your system node tooopen hello context menu, and then click **New** > **Virtual Machine**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. <span data-ttu-id="8edbf-158">På hello **innan du börjar** av hello guiden Ny virtuell dator, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-158">On hello **Before you begin** page of hello New Virtual Machine Wizard, click **Next**.</span></span>
5. <span data-ttu-id="8edbf-159">På hello **ange namn och plats** anger en **namn** för din virtuella matrisen.</span><span class="sxs-lookup"><span data-stu-id="8edbf-159">On hello **Specify name and location** page, provide a **Name** for your virtual array.</span></span> <span data-ttu-id="8edbf-160">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-160">Click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. <span data-ttu-id="8edbf-161">På hello **ange generation** , Välj hello enhetstyp avbildningen, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-161">On hello **Specify generation** page, choose hello device image type, and then click **Next**.</span></span> <span data-ttu-id="8edbf-162">Den här sidan visas inte om du använder Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="8edbf-162">This page doesn't appear if you're using Windows Server 2008 R2.</span></span>

   * <span data-ttu-id="8edbf-163">Välj **Generation 2** om du har hämtat en vhdx-avbildning för Windows Server 2012 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8edbf-163">Choose **Generation 2** if you downloaded a .vhdx image for Windows Server 2012 or later.</span></span>
   * <span data-ttu-id="8edbf-164">Välj **Generation 1** om du har hämtat en VHD-avbildning för Windows Server 2008 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8edbf-164">Choose **Generation 1** if you downloaded a .vhd image for Windows Server 2008 R2 or later.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. <span data-ttu-id="8edbf-165">På hello **tilldela minne** anger en **startminne** av minst **8192 MB**inte aktivera dynamiskt minne, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-165">On hello **Assign memory** page, specify a **Startup memory** of at least **8192 MB**, don't enable dynamic memory, and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. <span data-ttu-id="8edbf-166">På hello **Konfigurera nätverk** anger hello virtuell växel som är anslutna toohello Internet och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-166">On hello **Configure networking** page, specify hello virtual switch that is connected toohello Internet and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. <span data-ttu-id="8edbf-167">På hello **Anslut virtuell hårddisk** väljer **använder en befintlig virtuell hårddisk**anger hello plats hello virtuella matris bild (vhdx eller VHD) och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-167">On hello **Connect virtual hard disk** page, choose **Use an existing virtual hard disk**, specify hello location of hello virtual array image (.vhdx or .vhd), and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. <span data-ttu-id="8edbf-168">Granska hello **sammanfattning** och klicka sedan på **Slutför** toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8edbf-168">Review hello **Summary** and then click **Finish** toocreate hello virtual machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. <span data-ttu-id="8edbf-169">toomeet hello minimikraven, behöver du 4 kärnor.</span><span class="sxs-lookup"><span data-stu-id="8edbf-169">toomeet hello minimum requirements, you need 4 cores.</span></span> <span data-ttu-id="8edbf-170">tooadd 4 virtuella processorer, Välj din värdsystem i hello **Hyper-V Manager** fönster.</span><span class="sxs-lookup"><span data-stu-id="8edbf-170">tooadd 4 virtual processors, select your host system in hello **Hyper-V Manager** window.</span></span> <span data-ttu-id="8edbf-171">I hello högra rutan under hello lista över **virtuella datorer**, leta upp hello virtuell dator som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="8edbf-171">In hello right-pane under hello list of **Virtual Machines**, locate hello virtual machine you just created.</span></span> <span data-ttu-id="8edbf-172">Markera och högerklicka på datornamnet hello och välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-172">Select and right-click hello machine name and select **Settings**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. <span data-ttu-id="8edbf-173">På hello **inställningar** i hello vänstra fönsterrutan klickar du på **Processor**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-173">On hello **Settings** page, in hello left-pane, click **Processor**.</span></span> <span data-ttu-id="8edbf-174">Hello högra rutan Ange **antal virtuella processorer** too4 (eller mer).</span><span class="sxs-lookup"><span data-stu-id="8edbf-174">In hello right-pane, set **number of virtual processors** too4 (or more).</span></span> <span data-ttu-id="8edbf-175">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-175">Click **Apply**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. <span data-ttu-id="8edbf-176">toomeet hello minimikraven, behöver du också tooadd en 500 GB data för virtuell disk.</span><span class="sxs-lookup"><span data-stu-id="8edbf-176">toomeet hello minimum requirements, you also need tooadd a 500 GB virtual data disk.</span></span> <span data-ttu-id="8edbf-177">I hello **inställningar** sidan:</span><span class="sxs-lookup"><span data-stu-id="8edbf-177">In hello **Settings** page:</span></span>

    1. <span data-ttu-id="8edbf-178">I hello vänster och välj **SCSI-styrenhet**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-178">In hello left pane, select **SCSI Controller**.</span></span>
    2. <span data-ttu-id="8edbf-179">I hello högra rutan, Välj **hårddisk,** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-179">In hello right pane, select **Hard Drive,** and click **Add**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. <span data-ttu-id="8edbf-180">På hello **hårddisken** sidan, Välj hello **virtuella hårddisk** och klickar på **ny**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-180">On hello **Hard drive** page, select hello **Virtual hard disk** option and click **New**.</span></span> <span data-ttu-id="8edbf-181">Hej **guiden för ny virtuell hårddisk** startar.</span><span class="sxs-lookup"><span data-stu-id="8edbf-181">hello **New Virtual Hard Disk Wizard** starts.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. <span data-ttu-id="8edbf-182">På hello **innan du börjar** av hello guiden Ny virtuell hårddisk, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-182">On hello **Before you begin** page of hello New Virtual Hard Disk Wizard, click **Next**.</span></span>
16. <span data-ttu-id="8edbf-183">På hello **sidan Välj diskformat**, acceptera hello standardalternativet **VHDX** format.</span><span class="sxs-lookup"><span data-stu-id="8edbf-183">On hello **Choose Disk Format page**, accept hello default option of **VHDX** format.</span></span> <span data-ttu-id="8edbf-184">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-184">Click **Next**.</span></span> <span data-ttu-id="8edbf-185">Den här skärmen visas inte om du kör Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="8edbf-185">This screen is not presented if running Windows Server 2008 R2.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. <span data-ttu-id="8edbf-186">På hello **sidan Välj disktyp**, ange typen av virtuell hårddisk som **dynamiskt expanderande** (rekommenderas).</span><span class="sxs-lookup"><span data-stu-id="8edbf-186">On hello **Choose Disk Type page**, set virtual hard disk type as **Dynamically expanding** (recommended).</span></span> <span data-ttu-id="8edbf-187">**En fast storlek** disk skulle fungera men du kan behöva toowait lång tid.</span><span class="sxs-lookup"><span data-stu-id="8edbf-187">**Fixed size** disk would work but you may need toowait a long time.</span></span> <span data-ttu-id="8edbf-188">Vi rekommenderar att du inte använder hello **Differencing** alternativet.</span><span class="sxs-lookup"><span data-stu-id="8edbf-188">We recommend that you do not use hello **Differencing** option.</span></span> <span data-ttu-id="8edbf-189">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-189">Click **Next**.</span></span> <span data-ttu-id="8edbf-190">I Windows Server 2012 R2 och Windows Server 2012 **dynamiskt expanderande** är hello standardalternativet i Windows Server 2008 R2 standard hello är **fast storlek**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-190">In Windows Server 2012 R2 and Windows Server 2012, **Dynamically expanding** is hello default option whereas in Windows Server 2008 R2, hello default is **Fixed size**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. <span data-ttu-id="8edbf-191">På hello **ange namn och plats** anger en **namn** samt **plats** (du kan bläddra tooone) för hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="8edbf-191">On hello **Specify Name and Location** page, provide a **name** as well as **location** (you can browse tooone) for hello data disk.</span></span> <span data-ttu-id="8edbf-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-192">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. <span data-ttu-id="8edbf-193">På hello **konfigurera Disk** sidan Välj hello alternativet **skapa en ny tom virtuell hårddisk** och ange hello storlek som **500 GB** (eller mer).</span><span class="sxs-lookup"><span data-stu-id="8edbf-193">On hello **Configure Disk** page, select hello option **Create a new blank virtual hard disk** and specify hello size as **500 GB** (or more).</span></span> <span data-ttu-id="8edbf-194">500 GB är minimikravet för hello, kan du alltid etablera en större disk.</span><span class="sxs-lookup"><span data-stu-id="8edbf-194">While 500 GB is hello minimum requirement, you can always provision a larger disk.</span></span> <span data-ttu-id="8edbf-195">Observera att du kan öka eller minska hello-disk som etablerats en gång.</span><span class="sxs-lookup"><span data-stu-id="8edbf-195">Note that you cannot expand or shrink hello disk once provisioned.</span></span> <span data-ttu-id="8edbf-196">Mer information om hello storleken på disken tooprovision granska hello sizing avsnitt i hello [bästa praxis dokumentet](storsimple-ova-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="8edbf-196">For more information on hello size of disk tooprovision, review hello sizing section in hello [best practices document](storsimple-ova-best-practices.md).</span></span> <span data-ttu-id="8edbf-197">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-197">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. <span data-ttu-id="8edbf-198">På hello **sammanfattning** granskar hello information om din virtuella datadisk och om uppfyllt, klickar du på **Slutför** toocreate hello disk.</span><span class="sxs-lookup"><span data-stu-id="8edbf-198">On hello **Summary** page, review hello details of your virtual data disk and if satisfied, click **Finish** toocreate hello disk.</span></span> <span data-ttu-id="8edbf-199">hello guiden stängs och en virtuell hårddisk läggs tooyour datorn.</span><span class="sxs-lookup"><span data-stu-id="8edbf-199">hello wizard closes and a virtual hard disk is added tooyour machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. <span data-ttu-id="8edbf-200">Returnera toohello **inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="8edbf-200">Return toohello **Settings** page.</span></span> <span data-ttu-id="8edbf-201">Klicka på **OK** tooclose hello **inställningar** sidan och returnera tooHyper-V Manager-fönstret.</span><span class="sxs-lookup"><span data-stu-id="8edbf-201">Click **OK** tooclose hello **Settings** page and return tooHyper-V Manager window.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-hello-virtual-array-and-get-hello-ip"></a><span data-ttu-id="8edbf-202">Steg 3: Starta hello virtuella matris och hämta hello IP</span><span class="sxs-lookup"><span data-stu-id="8edbf-202">Step 3: Start hello virtual array and get hello IP</span></span>
<span data-ttu-id="8edbf-203">Utför följande steg toostart hello virtuella matrisen och ansluta tooit.</span><span class="sxs-lookup"><span data-stu-id="8edbf-203">Perform hello following steps toostart your virtual array and connect tooit.</span></span>

#### <a name="toostart-hello-virtual-array"></a><span data-ttu-id="8edbf-204">toostart hello virtuella matris</span><span class="sxs-lookup"><span data-stu-id="8edbf-204">toostart hello virtual array</span></span>
1. <span data-ttu-id="8edbf-205">Starta virtuella hello-matris.</span><span class="sxs-lookup"><span data-stu-id="8edbf-205">Start hello virtual array.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. <span data-ttu-id="8edbf-206">När hello enheten körs Välj hello enhet, högerklicka på och välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-206">After hello device is running, select hello device, right click, and select **Connect**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. <span data-ttu-id="8edbf-207">Du kan ha toowait 5-10 minuter för hello enheten toobe redo.</span><span class="sxs-lookup"><span data-stu-id="8edbf-207">You may have toowait 5-10 minutes for hello device toobe ready.</span></span> <span data-ttu-id="8edbf-208">Ett meddelande visas på hello konsolen tooindicate hello pågår.</span><span class="sxs-lookup"><span data-stu-id="8edbf-208">A status message is displayed on hello console tooindicate hello progress.</span></span> <span data-ttu-id="8edbf-209">När hello enheten är klar, går för**åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="8edbf-209">After hello device is ready, go too**Action**.</span></span> <span data-ttu-id="8edbf-210">Tryck på `Ctrl + Alt + Delete` toolog i toohello virtuella matris.</span><span class="sxs-lookup"><span data-stu-id="8edbf-210">Press `Ctrl + Alt + Delete` toolog in toohello virtual array.</span></span> <span data-ttu-id="8edbf-211">hello standardanvändaren är *StorSimpleAdmin* och hello standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="8edbf-211">hello default user is *StorSimpleAdmin* and hello default password is *Password1*.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. <span data-ttu-id="8edbf-212">Av säkerhetsskäl upphör hello enhetens administratörslösenord vid hello första inloggningen.</span><span class="sxs-lookup"><span data-stu-id="8edbf-212">For security reasons, hello device administrator password expires at hello first logon.</span></span> <span data-ttu-id="8edbf-213">Du kan ange toochange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="8edbf-213">You are prompted toochange hello password.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   <span data-ttu-id="8edbf-214">Ange ett lösenord som innehåller minst 8 tecken.</span><span class="sxs-lookup"><span data-stu-id="8edbf-214">Enter a password that contains at least 8 characters.</span></span> <span data-ttu-id="8edbf-215">hello lösenordet måste uppfylla minst 3 av följande 4 krav hello: versaler, gemener, siffror och särskilda tecken.</span><span class="sxs-lookup"><span data-stu-id="8edbf-215">hello password must satisfy at least 3 out of hello following 4 requirements: uppercase, lowercase, numeric, and special characters.</span></span> <span data-ttu-id="8edbf-216">Ange hello lösenord tooconfirm den.</span><span class="sxs-lookup"><span data-stu-id="8edbf-216">Reenter hello password tooconfirm it.</span></span> <span data-ttu-id="8edbf-217">Du meddelas hello lösenordet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="8edbf-217">You are notified that hello password has changed.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. <span data-ttu-id="8edbf-218">Hello virtuella matris får att starta om när hello lösenordet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="8edbf-218">After hello password is successfully changed, hello virtual array may restart.</span></span> <span data-ttu-id="8edbf-219">Vänta tills hello enheten toostart.</span><span class="sxs-lookup"><span data-stu-id="8edbf-219">Wait for hello device toostart.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    <span data-ttu-id="8edbf-220">hello Windows PowerShell-konsol för hello enhet visas tillsammans med en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="8edbf-220">hello Windows PowerShell console of hello device is displayed along with a progress bar.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. <span data-ttu-id="8edbf-221">Steg 6 – 8 gäller endast när du startar upp i en icke-DHCP-miljö.</span><span class="sxs-lookup"><span data-stu-id="8edbf-221">Steps 6-8 only apply when booting up in a non-DHCP environment.</span></span> <span data-ttu-id="8edbf-222">Om du har en DHCP-miljö, hoppa över de här stegen och går toostep 9.</span><span class="sxs-lookup"><span data-stu-id="8edbf-222">If you are in a DHCP environment, then skip these steps and go toostep 9.</span></span> <span data-ttu-id="8edbf-223">Om du har startat upp din enhet i DHCP-miljön visas hello följande skärm.</span><span class="sxs-lookup"><span data-stu-id="8edbf-223">If you booted up your device in non-DHCP environment, you will see hello following screen.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    <span data-ttu-id="8edbf-224">Konfigurera hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="8edbf-224">Next, configure hello network.</span></span>
7. <span data-ttu-id="8edbf-225">Använd hello `Get-HcsIpAddress` kommandot toolist hello nätverksgränssnitt aktiverade på din virtuella matrisen.</span><span class="sxs-lookup"><span data-stu-id="8edbf-225">Use hello `Get-HcsIpAddress` command toolist hello network interfaces enabled on your virtual array.</span></span> <span data-ttu-id="8edbf-226">Om enheten har ett enda nätverksgränssnitt som är aktiverad, hello standardgränssnittet tilldelade toothis är `Ethernet`.</span><span class="sxs-lookup"><span data-stu-id="8edbf-226">If your device has a single network interface enabled, hello default name assigned toothis interface is `Ethernet`.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. <span data-ttu-id="8edbf-227">Använd hello `Set-HcsIpAddress` cmdlet tooconfigure hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="8edbf-227">Use hello `Set-HcsIpAddress` cmdlet tooconfigure hello network.</span></span> <span data-ttu-id="8edbf-228">Se följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8edbf-228">See hello following example:</span></span>

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. <span data-ttu-id="8edbf-229">När hello installationen är klar och hello enheten har startats in, visas hello banderolltext för enheten.</span><span class="sxs-lookup"><span data-stu-id="8edbf-229">After hello initial setup is complete and hello device has booted up, you will see hello device banner text.</span></span> <span data-ttu-id="8edbf-230">Anteckna hello IP-adress och hello-URL som visas i hello banderoll text toomanage hello enhet.</span><span class="sxs-lookup"><span data-stu-id="8edbf-230">Make a note of hello IP address and hello URL displayed in hello banner text toomanage hello device.</span></span> <span data-ttu-id="8edbf-231">Använd den här IP-adressen tooconnect toohello webbgränssnittet för ditt virtuella matris och fullständig hello lokal installation och registrering.</span><span class="sxs-lookup"><span data-stu-id="8edbf-231">Use this IP address tooconnect toohello web UI of your virtual array and complete hello local setup and registration.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. <span data-ttu-id="8edbf-232">(Valfritt) Utför det här steget endast om du distribuerar din enhet i hello offentliga moln.</span><span class="sxs-lookup"><span data-stu-id="8edbf-232">(Optional) Perform this step only if you are deploying your device in hello Government Cloud.</span></span> <span data-ttu-id="8edbf-233">Du kan nu hello USA FIPS Federal Information Processing Standard ()-läge på enheten.</span><span class="sxs-lookup"><span data-stu-id="8edbf-233">You will now enable hello United States Federal Information Processing Standard (FIPS) mode on your device.</span></span> <span data-ttu-id="8edbf-234">hello FIPS 140-standarden definierar kryptografiska algoritmer som godkänts för användning av oss federala myndigheter datorsystem för hello skyddet av känsliga data.</span><span class="sxs-lookup"><span data-stu-id="8edbf-234">hello FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for hello protection of sensitive data.</span></span>

    1. <span data-ttu-id="8edbf-235">tooenable hello FIPS-läge, kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="8edbf-235">tooenable hello FIPS mode, run hello following cmdlet:</span></span>

        `Enable-HcsFIPSMode`
    2. <span data-ttu-id="8edbf-236">Starta om enheten när du har aktiverat hello FIPS-läge så att hello kryptografiska verifieringar börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="8edbf-236">Reboot your device after you have enabled hello FIPS mode so that hello cryptographic validations take effect.</span></span>

       > [!NOTE]
       > <span data-ttu-id="8edbf-237">Du kan aktivera eller inaktivera FIPS-läge på enheten.</span><span class="sxs-lookup"><span data-stu-id="8edbf-237">You can either enable or disable FIPS mode on your device.</span></span> <span data-ttu-id="8edbf-238">Alternerande hello enhet mellan FIPS och icke-FIPS-läge stöds inte.</span><span class="sxs-lookup"><span data-stu-id="8edbf-238">Alternating hello device between FIPS and non-FIPS mode is not supported.</span></span>
       >
       >

<span data-ttu-id="8edbf-239">Om enheten inte uppfyller lägsta konfigurationskrav som hello finns hello följande fel i hello banderolltext (se nedan).</span><span class="sxs-lookup"><span data-stu-id="8edbf-239">If your device does not meet hello minimum configuration requirements, you see hello following error in hello banner text (shown below).</span></span> <span data-ttu-id="8edbf-240">Ändra hello enhetskonfigurationen så att hello datorn har tillräckliga resurser toomeet hello minimikraven.</span><span class="sxs-lookup"><span data-stu-id="8edbf-240">Modify hello device configuration so that hello machine has adequate resources toomeet hello minimum requirements.</span></span> <span data-ttu-id="8edbf-241">Du kan sedan starta om och Anslut toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="8edbf-241">You can then restart and connect toohello device.</span></span> <span data-ttu-id="8edbf-242">Se toohello lägsta konfigurationskrav i [steg 1: Kontrollera att hello värdsystem uppfyller minsta virtuella matris](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span><span class="sxs-lookup"><span data-stu-id="8edbf-242">Refer toohello minimum configuration requirements in [Step 1: Ensure that hello host system meets minimum virtual array requirements](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span></span>

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

<span data-ttu-id="8edbf-243">Om du står inför ett fel under hello startkonfiguration med hello lokala webbgränssnittet Se toohello följande arbetsflöden:</span><span class="sxs-lookup"><span data-stu-id="8edbf-243">If you face any other error during hello initial configuration using hello local web UI, refer toohello following workflows:</span></span>

* <span data-ttu-id="8edbf-244">Kör diagnostiktest för[felsöka installationen av UI](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span><span class="sxs-lookup"><span data-stu-id="8edbf-244">Run diagnostic tests too[troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span></span>
* <span data-ttu-id="8edbf-245">[Generera loggen paketet och visa loggfiler](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="8edbf-245">[Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8edbf-246">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8edbf-246">Next steps</span></span>
* [<span data-ttu-id="8edbf-247">Konfigurera din virtuella StorSimple-matris som en filserver</span><span class="sxs-lookup"><span data-stu-id="8edbf-247">Set up your StorSimple Virtual Array as a file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)
* [<span data-ttu-id="8edbf-248">Konfigurera din virtuella StorSimple-matris som en iSCSI-server</span><span class="sxs-lookup"><span data-stu-id="8edbf-248">Set up your StorSimple Virtual Array as an iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md)
