<!--author=alkohli last changed: 07/07/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a><span data-ttu-id="fae48-101">Installera en uppdatering från Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fae48-101">To install an update from the Azure portal</span></span>

1. <span data-ttu-id="fae48-102">Välj enheten på tjänstesidan StorSimple.</span><span class="sxs-lookup"><span data-stu-id="fae48-102">On the StorSimple service page, select your device.</span></span>

    ![Välj enhet](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. <span data-ttu-id="fae48-104">Gå till **Enhetsinställningar** > **enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="fae48-104">Navigate to **Device settings** > **Device updates**.</span></span>

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. <span data-ttu-id="fae48-106">Ett meddelande visas om det finns nya uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="fae48-106">A notification appears if new updates are available.</span></span> <span data-ttu-id="fae48-107">Du kan också i den **enhetsuppdateringar** bladet klickar du på **Sök efter uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="fae48-107">Alternatively, in the **Device updates** blade, click **Scan Updates**.</span></span> <span data-ttu-id="fae48-108">Det skapas ett jobb för att söka efter tillgängliga uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="fae48-108">A job is created to scan for available updates.</span></span> <span data-ttu-id="fae48-109">Du meddelas när jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fae48-109">You are notified when the job completes successfully.</span></span>

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. <span data-ttu-id="fae48-111">Vi rekommenderar att du läser den viktiga informationen innan du installerar en uppdatering på enheten.</span><span class="sxs-lookup"><span data-stu-id="fae48-111">We recommend that you review the release notes before you apply an update on your device.</span></span> <span data-ttu-id="fae48-112">Om du vill tillämpa uppdateringar, klickar du på **installera uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="fae48-112">To apply updates, click **Install updates**.</span></span> <span data-ttu-id="fae48-113">I den **bekräfta regelbundna uppdateringar** bladet granska förutsättningar för att kunna slutföra innan du installerar uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="fae48-113">In the **Confirm regular updates** blade, review the prerequisites to complete before you apply updates.</span></span> <span data-ttu-id="fae48-114">Markera kryssrutan för att visa att du är redo att uppdatera enheten och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="fae48-114">Select the checkbox to indicate that you are ready to update the device and then click **Install**.</span></span>

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. <span data-ttu-id="fae48-116">En uppsättning nödvändiga kontrollerar startar.</span><span class="sxs-lookup"><span data-stu-id="fae48-116">A set of prerequisite checks starts.</span></span> <span data-ttu-id="fae48-117">Dessa kontroller omfattar följande:</span><span class="sxs-lookup"><span data-stu-id="fae48-117">These checks include:</span></span>
   
   * <span data-ttu-id="fae48-118">**Hälsokontroller av styrenheten** för att verifiera att båda styrenheterna är felfria och online.</span><span class="sxs-lookup"><span data-stu-id="fae48-118">**Controller health checks** to verify that both the device controllers are healthy and online.</span></span>
   * <span data-ttu-id="fae48-119">**Hälsokontroller för maskinvarukomponenter** för att verifiera att alla maskinvarukomponenter på StorSimple-enheten är felfria.</span><span class="sxs-lookup"><span data-stu-id="fae48-119">**Hardware component health checks** to verify that all the hardware components on your StorSimple device are healthy.</span></span>
   * <span data-ttu-id="fae48-120">**DATA 0-kontroller** för att verifiera att DATA 0 är aktiverat på enheten.</span><span class="sxs-lookup"><span data-stu-id="fae48-120">**DATA 0 checks** to verify that DATA 0 is enabled on your device.</span></span> <span data-ttu-id="fae48-121">Om inte det här gränssnittet är aktiverat måste du aktivera det och sedan försöka igen.</span><span class="sxs-lookup"><span data-stu-id="fae48-121">If this interface is not enabled, you must enable it and then retry.</span></span>

    <span data-ttu-id="fae48-122">Uppdateringen hämtas och installeras bara om alla kontroller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fae48-122">The update is downloaded and installed only if all the checks are successfully completed.</span></span> <span data-ttu-id="fae48-123">Du får ett meddelande när kontrollerna pågår.</span><span class="sxs-lookup"><span data-stu-id="fae48-123">You are notified when the checks are in progress.</span></span> <span data-ttu-id="fae48-124">Om prechecks misslyckas sedan anges du med orsakerna till felet.</span><span class="sxs-lookup"><span data-stu-id="fae48-124">If the prechecks fail, then you will be provided with the reasons for failure.</span></span> <span data-ttu-id="fae48-125">Lösa dessa problem och försök sedan igen.</span><span class="sxs-lookup"><span data-stu-id="fae48-125">Address those issues and then retry the operation.</span></span> <span data-ttu-id="fae48-126">Du kan behöva kontakta Microsofts support om du inte kan lösa dessa problem själv.</span><span class="sxs-lookup"><span data-stu-id="fae48-126">You may need to contact Microsoft Support if you cannot address these issues by yourself.</span></span>

7. <span data-ttu-id="fae48-127">När prechecks har slutförts, skapas ett uppdateringsjobb.</span><span class="sxs-lookup"><span data-stu-id="fae48-127">After the prechecks are successfully completed, an update job is created.</span></span> <span data-ttu-id="fae48-128">Du får ett meddelande när uppdateringsjobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="fae48-128">You are notified when the update job is successfully created.</span></span>
   
    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    <span data-ttu-id="fae48-130">Uppdateringen tillämpas sedan på enheten.</span><span class="sxs-lookup"><span data-stu-id="fae48-130">The update is then applied on your device.</span></span>

9. <span data-ttu-id="fae48-131">Uppdateringen tar några timmar att slutföra.</span><span class="sxs-lookup"><span data-stu-id="fae48-131">The update takes a few hours to complete.</span></span> <span data-ttu-id="fae48-132">Markera uppdateringsjobbet och klicka på **Information** så kan du visa information om jobbet när som helst.</span><span class="sxs-lookup"><span data-stu-id="fae48-132">Select the update job and click **Details** to view the details of the job at any time.</span></span>

    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update8.png)

     <span data-ttu-id="fae48-134">Du kan också övervaka förloppet för Uppdateringsjobbet från **Enhetsinställningar > jobb**.</span><span class="sxs-lookup"><span data-stu-id="fae48-134">You can also monitor the progress of the update job from **Device settings > Jobs**.</span></span> <span data-ttu-id="fae48-135">På den **jobb** bladet du kan se förloppet för uppdatering.</span><span class="sxs-lookup"><span data-stu-id="fae48-135">On the **Jobs** blade, you can see the update progress.</span></span>

     ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. <span data-ttu-id="fae48-137">När jobbet har slutförts, går du till den **Enhetsinställningar > enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="fae48-137">After the job is complete, navigate to the **Device settings > Device updates**.</span></span> <span data-ttu-id="fae48-138">Programvaruversionen ska nu uppdateras.</span><span class="sxs-lookup"><span data-stu-id="fae48-138">The software version should now be updated.</span></span>

    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update9.png)

