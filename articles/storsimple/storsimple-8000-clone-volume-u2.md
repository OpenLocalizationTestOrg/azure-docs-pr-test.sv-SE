---
title: "aaaClone en volym på StorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hello klona av olika typer och användning och förklarar hur du kan använda en säkerhetskopia tooclone en enskild volym på en enhet för StorSimple 8000-serien."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 4f7e1f62f17c7b2bd72820a00a5ab87b7e192332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-tooclone-a-volume"></a><span data-ttu-id="2fc5e-103">Använd hello StorSimple enheten Manager-tjänsten i Azure portal tooclone en volym</span><span class="sxs-lookup"><span data-stu-id="2fc5e-103">Use hello StorSimple Device Manager service in Azure portal tooclone a volume</span></span>

## <a name="overview"></a><span data-ttu-id="2fc5e-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2fc5e-104">Overview</span></span>

<span data-ttu-id="2fc5e-105">Den här självstudiekursen beskriver hur du kan använda en säkerhetskopia tooclone en enskild volym via hello **säkerhetskopieringskatalog** bladet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-105">This tutorial describes how you can use a backup set tooclone an individual volume via hello **Backup catalog** blade.</span></span> <span data-ttu-id="2fc5e-106">Här förklaras också hello skillnaden mellan *tillfälligt* och *permanent* klonar.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-106">It also explains hello difference between *transient* and *permanent* clones.</span></span> <span data-ttu-id="2fc5e-107">hello anvisningarna i den här kursen gäller tooall hello StorSimple 8000-serieenhet som kör uppdatering 3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-107">hello guidance in this tutorial applies tooall hello StorSimple 8000 series device running Update 3 or later.</span></span>

<span data-ttu-id="2fc5e-108">Hej StorSimple Enhetshanteraren service **säkerhetskopieringskatalog** bladet visar alla hello säkerhetskopior som skapas när manuell eller automatisk säkerhetskopiering utförs.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-108">hello StorSimple Device Manager service **Backup catalog** blade displays all hello backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="2fc5e-109">Du kan sedan välja en volym i en säkerhetskopia tooclone.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-109">You can then select a volume in a backup set tooclone.</span></span>

 ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a><span data-ttu-id="2fc5e-111">Överväganden för kloning av en volym</span><span class="sxs-lookup"><span data-stu-id="2fc5e-111">Considerations for cloning a volume</span></span>

<span data-ttu-id="2fc5e-112">Överväg att hello följande information när du klonar en volym.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-112">Consider hello following information when cloning a volume.</span></span>

- <span data-ttu-id="2fc5e-113">En klon fungerar i hello samma sätt som en vanlig volym.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-113">A clone behaves in hello same way as a regular volume.</span></span> <span data-ttu-id="2fc5e-114">Alla åtgärder som är möjligt på en volym är tillgänglig för hello kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-114">Any operation that is possible on a volume is available for hello clone.</span></span>

- <span data-ttu-id="2fc5e-115">Övervakning och standard säkerhetskopieringen inaktiveras automatiskt på en klonade volym.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-115">Monitoring and default backup are automatically disabled on a cloned volume.</span></span> <span data-ttu-id="2fc5e-116">Du behöver tooconfigure klonade volym för säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-116">You would need tooconfigure a cloned volume for any backups.</span></span>

- <span data-ttu-id="2fc5e-117">En lokalt Fäst volym klonas som en nivåindelad volym.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-117">A locally pinned volume is cloned as a tiered volume.</span></span> <span data-ttu-id="2fc5e-118">Om du behöver hello klonade volym toobe lokalt Fäst, kan du konvertera hello klona tooa lokalt Fäst volym när hello kopieringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-118">If you need hello cloned volume toobe locally pinned, you can convert hello clone tooa locally pinned volume after hello clone operation is successfully completed.</span></span> <span data-ttu-id="2fc5e-119">För information om hur du konverterar en nivåindelad volym tooa lokalt Fäst volym, gå för[ändra hello volymtyp](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="2fc5e-119">For information about converting a tiered volume tooa locally pinned volume, go too[Change hello volume type](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span></span>

- <span data-ttu-id="2fc5e-120">Om du försöker tooconvert en klonade volym från nivåindelade toolocally Fäst omedelbart efter att Kloningen (när det är fortfarande ett tillfälligt klona), misslyckas hello konvertering med hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="2fc5e-120">If you try tooconvert a cloned volume from tiered toolocally pinned immediately after cloning (when it is still a transient clone), hello conversion fails with hello following error message:</span></span>

    `Unable toomodify hello usage type for volume {0}. This can happen if hello volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry hello modify operation.`

    <span data-ttu-id="2fc5e-121">Det här felmeddelandet endast om du klona på tooa annan enhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-121">This error is received only if you are cloning on tooa different device.</span></span> <span data-ttu-id="2fc5e-122">Du konverterar hello volym toolocally Fäst om du först konverterar hello tillfälligt klona tooa permanent kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-122">You can successfully convert hello volume toolocally pinned if you first convert hello transient clone tooa permanent clone.</span></span> <span data-ttu-id="2fc5e-123">Ta en ögonblicksbild i molnet av hello tillfälligt klona tooconvert den tooa permanent kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-123">Take a cloud snapshot of hello transient clone tooconvert it tooa permanent clone.</span></span>

## <a name="create-a-clone-of-a-volume"></a><span data-ttu-id="2fc5e-124">Skapa en klon av en volym</span><span class="sxs-lookup"><span data-stu-id="2fc5e-124">Create a clone of a volume</span></span>

<span data-ttu-id="2fc5e-125">Du kan skapa en klon på hello samma enhet, en annan enhet eller även en moln-installation med hjälp av en lokal eller molnbaserade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-125">You can create a clone on hello same device, another device, or even a cloud appliance by using a local or cloud snapshot.</span></span>

<span data-ttu-id="2fc5e-126">hello beskrivs nedan hur toocreate en klon från hello säkerhetskopiera katalog.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-126">hello procedure below describes how toocreate a clone from hello backup catalog.</span></span>  <span data-ttu-id="2fc5e-127">En alternativ metod tooinitiate klona är toogo för**volymer**, väljer du en volym, högerklicka på tooinvoke hello snabbmenyn och välj **klona**.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-127">An alternative method tooinitiate clone is toogo too**Volumes**, select a volume, then right-click tooinvoke hello context menu and select **Clone**.</span></span>

<span data-ttu-id="2fc5e-128">Utför följande steg toocreate en klon av volymen från hello säkerhetskopieringskatalogen hello.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-128">Perform hello following steps toocreate a clone of your volume from hello backup catalog.</span></span>

#### <a name="tooclone-a-volume"></a><span data-ttu-id="2fc5e-129">tooclone en volym</span><span class="sxs-lookup"><span data-stu-id="2fc5e-129">tooclone a volume</span></span>

1. <span data-ttu-id="2fc5e-130">Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-130">Go tooyour StorSimple Device Manager service and then click **Backup catalog**.</span></span>

2. <span data-ttu-id="2fc5e-131">Välj en säkerhetskopia på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2fc5e-131">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="2fc5e-132">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-132">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="2fc5e-133">Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-133">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="2fc5e-134">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-134">Specify hello time range.</span></span>
   4. <span data-ttu-id="2fc5e-135">Klicka på **tillämpa** tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-135">Click **Apply** tooexecute this query.</span></span>

    <span data-ttu-id="2fc5e-136">hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-136">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
   
    ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. <span data-ttu-id="2fc5e-138">Expandera hello säkerhetskopia tooview hello associerade volymer.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-138">Expand hello backup set tooview hello associated volumes.</span></span> <span data-ttu-id="2fc5e-139">Dessa volymer måste vara offline på hello värd och enheten innan du kan återställa dem.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-139">These volumes must be taken offline on hello host and device before you can restore them.</span></span> <span data-ttu-id="2fc5e-140">Komma åt hello volymer på hello **volymer** bladet för din enhet och hello Följ stegen i [kopplar från en volym](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake dem offline.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-140">Access hello volumes on hello **Volumes** blade of your device, and then follow hello steps in [Take a volume offline](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake them offline.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="2fc5e-141">Kontrollera att du har vidtagit hello volymer offline på värden för hello först innan du utför hello volymer på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-141">Make sure that you have taken hello volumes offline on hello host first, before you take hello volumes offline on hello device.</span></span> <span data-ttu-id="2fc5e-142">Om du inte vidtar hello volymer offline på värden för hello leda potentiellt toodata skadas.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-142">If you do not take hello volumes offline on hello host, it could potentially lead toodata corruption.</span></span>
   
4. <span data-ttu-id="2fc5e-143">Gå tillbaka toohello **säkerhetskopieringskatalog** och välj en volym i en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-143">Navigate back toohello **Backup catalog** and select a volume in a backup set.</span></span> <span data-ttu-id="2fc5e-144">Högerklicka och välj sedan hello snabbmenyn **klona**.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-144">Right-click and then from hello context menu, select **Clone**.</span></span>

   ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. <span data-ttu-id="2fc5e-146">I hello **klona** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2fc5e-146">In hello **Clone** blade, do hello following steps:</span></span>
   
    1. <span data-ttu-id="2fc5e-147">Identifiera en målenhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-147">Identify a target device.</span></span> <span data-ttu-id="2fc5e-148">Detta är hello plats där hello klona kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-148">This is hello location where hello clone will be created.</span></span> <span data-ttu-id="2fc5e-149">Du kan välja hello samma enhet eller ange en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-149">You can choose hello same device or specify another device.</span></span>

      > [!NOTE]
      > <span data-ttu-id="2fc5e-150">Kontrollera att hello kapacitet som krävs för hello klona är lägre än hello kapacitet på hello målenhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-150">Make sure that hello capacity required for hello clone is lower than hello capacity available on hello target device.</span></span>
       
    2. <span data-ttu-id="2fc5e-151">Ange ett unikt volymnamn för din kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-151">Specify a unique volume name for your clone.</span></span> <span data-ttu-id="2fc5e-152">hello-namnet måste innehålla mellan 3 och 127 tecken.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-152">hello name must contain between 3 and 127 characters.</span></span>
      
        > [!NOTE]
        > <span data-ttu-id="2fc5e-153">Hej **klona volymen som** fältet kommer att vara **skiktindelade** även om kloning av en lokalt Fäst volym.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-153">hello **Clone Volume As** field will be **Tiered** even if you are cloning a locally pinned volume.</span></span> <span data-ttu-id="2fc5e-154">Du kan inte ändra den här inställningen. Om du behöver hello klonade volym toobe lokalt Fäst samt, kan du konvertera hello klona tooa lokalt Fäst volym när du har skapat hello kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-154">You cannot change this setting; however, if you need hello cloned volume toobe locally pinned as well, you can convert hello clone tooa locally pinned volume after you successfully create hello clone.</span></span> <span data-ttu-id="2fc5e-155">För information om hur du konverterar en nivåindelad volym tooa lokalt Fäst volym, gå för[ändra hello volymtyp](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="2fc5e-155">For information about converting a tiered volume tooa locally pinned volume, go too[Change hello volume type](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span></span>
          
    3. <span data-ttu-id="2fc5e-156">Under **anslutna värdar**, ange en åtkomstkontrollpost (ACR) för hello kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-156">Under **Connected hosts**, specify an access control record (ACR) for hello clone.</span></span> <span data-ttu-id="2fc5e-157">Du kan lägga till en ny ACR eller välj hello befintlig lista.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-157">You can add a new ACR or choose from hello existing list.</span></span> <span data-ttu-id="2fc5e-158">Hej ACR avgör vilka värdar som har åtkomst till den här klonen.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-158">hello ACR will determine which hosts can access this clone.</span></span>
      
        ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. <span data-ttu-id="2fc5e-160">Klicka på **klona** toocomplete hello igen.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-160">Click **Clone** toocomplete hello operation.</span></span>

4. <span data-ttu-id="2fc5e-161">Ett jobb för klon initieras och du meddelas när hello klona har skapats.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-161">A clone job is initiated and you are notified when hello clone is successfully created.</span></span> <span data-ttu-id="2fc5e-162">Klicka på meddelandet om hello eller gå för**jobb** bladet toomonitor hello klona jobb.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-162">Click hello job notification or go too**Jobs** blade toomonitor hello clone job.</span></span>

    ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. <span data-ttu-id="2fc5e-164">När hello klona jobbet har slutförts, går tooyour enheten och klickar på **volymer**.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-164">After hello clone job is complete, go tooyour device and then click **Volumes**.</span></span> <span data-ttu-id="2fc5e-165">I hello lista över volymer, bör du se hello klon som just skapades i hello samma volymbehållare som har hello källvolymen.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-165">In hello list of volumes, you should see hello clone that was just created in hello same volume container that has hello source volume.</span></span>

    ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

<span data-ttu-id="2fc5e-167">En klon som har skapats på detta sätt är ett tillfälligt kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-167">A clone that is created this way is a transient clone.</span></span> <span data-ttu-id="2fc5e-168">Mer information om kloning typer finns [tillfälliga och permanenta kloner](#transient-vs-permanent-clones).</span><span class="sxs-lookup"><span data-stu-id="2fc5e-168">For more information about clone types, see [Transient vs. permanent clones](#transient-vs-permanent-clones).</span></span>


## <a name="transient-vs-permanent-clones"></a><span data-ttu-id="2fc5e-169">Tillfälligt kontra permanent kloner</span><span class="sxs-lookup"><span data-stu-id="2fc5e-169">Transient vs. permanent clones</span></span>
<span data-ttu-id="2fc5e-170">Tillfälligt kloner skapas bara när du klonar tooanother enhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-170">Transient clones are created only when you clone tooanother device.</span></span> <span data-ttu-id="2fc5e-171">Du kan också klona en viss volym från en säkerhetskopia tooa annan enhet som hanteras av hello StorSimple Enhetshanteraren.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-171">You can clone a specific volume from a backup set tooa different device managed by hello StorSimple Device Manager.</span></span> <span data-ttu-id="2fc5e-172">hello tillfälligt klona har referenser toohello data i hello ursprungliga volymen och att data tooread och skriva lokalt på hello målenhet.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-172">hello transient clone has references toohello data in hello original volume and uses that data tooread and write locally on hello target device.</span></span>

<span data-ttu-id="2fc5e-173">När du har tagit en ögonblicksbild i molnet med tillfälliga klonen hello resulterande klon är en *permanent* kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-173">After you take a cloud snapshot of a transient clone, hello resulting clone is a *permanent* clone.</span></span> <span data-ttu-id="2fc5e-174">Under den här processen skapas på hello molnet en kopia av hello data och hello tid toocopy data bestäms av hello storleken på hello data och hello Azure latens (detta är en kopia av Azure till Azure).</span><span class="sxs-lookup"><span data-stu-id="2fc5e-174">During this process, a copy of hello data is created on hello cloud and hello time toocopy this data is determined by hello size of hello data and hello Azure latencies (this is an Azure-to-Azure copy).</span></span> <span data-ttu-id="2fc5e-175">Den här processen kan ta tooweeks dagar.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-175">This process can take days tooweeks.</span></span> <span data-ttu-id="2fc5e-176">hello tillfälligt klona blir en permanent kloning och innehåller inte några referenser toohello ursprungliga volymdata som den har klonats från.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-176">hello transient clone becomes a permanent clone and doesn’t have any references toohello original volume data that it was cloned from.</span></span>

## <a name="scenarios-for-transient-and-permanent-clones"></a><span data-ttu-id="2fc5e-177">Scenarier för tillfälliga och permanenta kloner</span><span class="sxs-lookup"><span data-stu-id="2fc5e-177">Scenarios for transient and permanent clones</span></span>
<span data-ttu-id="2fc5e-178">hello följande avsnitt beskrivs exempel situationer där tillfälliga och permanenta kloner kan användas.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-178">hello following sections describe example situations in which transient and permanent clones can be used.</span></span>

### <a name="item-level-recovery-with-a-transient-clone"></a><span data-ttu-id="2fc5e-179">Återställning på objektnivå med en tillfälliga klon</span><span class="sxs-lookup"><span data-stu-id="2fc5e-179">Item-level recovery with a transient clone</span></span>
<span data-ttu-id="2fc5e-180">Du måste toorecover en en-år-gamla Microsoft PowerPoint-fil.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-180">You need toorecover a one-year-old Microsoft PowerPoint presentation file.</span></span> <span data-ttu-id="2fc5e-181">IT-administratören identifierar hello säkerhetskopieringen från den tidpunkten och sedan filter hello volym.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-181">Your IT administrator identifies hello specific backup from that time, and then filters hello volume.</span></span> <span data-ttu-id="2fc5e-182">Hej administratör sedan klonar volymen hello hittar hello-fil som du söker efter och ger den tooyou.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-182">hello administrator then clones hello volume, locates hello file that you are looking for, and provides it tooyou.</span></span> <span data-ttu-id="2fc5e-183">I det här scenariot används ett tillfälligt kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-183">In this scenario, a transient clone is used.</span></span>

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a><span data-ttu-id="2fc5e-184">Testa i produktionsmiljö för hello med en permanent klon</span><span class="sxs-lookup"><span data-stu-id="2fc5e-184">Testing in hello production environment with a permanent clone</span></span>
<span data-ttu-id="2fc5e-185">Du måste tooverify ett tester programfel i hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-185">You need tooverify a testing bug in hello production environment.</span></span> <span data-ttu-id="2fc5e-186">Du skapar en klon av hello volymen i hello produktionsmiljö och ta en ögonblicksbild i molnet för den här klonen toocreate en oberoende klonade volym.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-186">You create a clone of hello volume in hello production environment and then take a cloud snapshot of this clone toocreate an independent cloned volume.</span></span> <span data-ttu-id="2fc5e-187">I det här scenariot används en permanent kloning.</span><span class="sxs-lookup"><span data-stu-id="2fc5e-187">In this scenario, a permanent clone is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fc5e-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fc5e-188">Next steps</span></span>
* <span data-ttu-id="2fc5e-189">Lär dig hur för[återställer en StorSimple-volym från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="2fc5e-189">Learn how too[restore a StorSimple volume from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="2fc5e-190">Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2fc5e-190">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

