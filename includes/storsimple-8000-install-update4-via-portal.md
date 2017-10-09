<!--author=alkohli last changed: 07/07/17-->

#### <a name="tooinstall-an-update-from-hello-azure-portal"></a><span data-ttu-id="d897b-101">tooinstall en uppdatering från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d897b-101">tooinstall an update from hello Azure portal</span></span>

1. <span data-ttu-id="d897b-102">Välj enheten hello StorSimple-tjänsten på sidan.</span><span class="sxs-lookup"><span data-stu-id="d897b-102">On hello StorSimple service page, select your device.</span></span>

    ![Välj enhet](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. <span data-ttu-id="d897b-104">Navigera för**Enhetsinställningar** > **enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="d897b-104">Navigate too**Device settings** > **Device updates**.</span></span>

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. <span data-ttu-id="d897b-106">Ett meddelande visas om det finns nya uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d897b-106">A notification appears if new updates are available.</span></span> <span data-ttu-id="d897b-107">Du kan också i hello **enhetsuppdateringar** bladet, klickar du på **Sök efter uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="d897b-107">Alternatively, in hello **Device updates** blade, click **Scan Updates**.</span></span> <span data-ttu-id="d897b-108">Ett jobb skapas tooscan efter uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d897b-108">A job is created tooscan for available updates.</span></span> <span data-ttu-id="d897b-109">Du meddelas när hello jobbet slutförs ordentligt.</span><span class="sxs-lookup"><span data-stu-id="d897b-109">You are notified when hello job completes successfully.</span></span>

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. <span data-ttu-id="d897b-111">Vi rekommenderar att du läser hello viktig information innan du installerar en uppdatering på enheten.</span><span class="sxs-lookup"><span data-stu-id="d897b-111">We recommend that you review hello release notes before you apply an update on your device.</span></span> <span data-ttu-id="d897b-112">tooapply uppdateringar, klickar du på **installera uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="d897b-112">tooapply updates, click **Install updates**.</span></span> <span data-ttu-id="d897b-113">I hello **bekräfta regelbundna uppdateringar** bladet granska hello krav toocomplete innan du installerar uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d897b-113">In hello **Confirm regular updates** blade, review hello prerequisites toocomplete before you apply updates.</span></span> <span data-ttu-id="d897b-114">Markera kryssrutan hello tooindicate att du är klar tooupdate hello enheten och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="d897b-114">Select hello checkbox tooindicate that you are ready tooupdate hello device and then click **Install**.</span></span>

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. <span data-ttu-id="d897b-116">En uppsättning nödvändiga kontrollerar startar.</span><span class="sxs-lookup"><span data-stu-id="d897b-116">A set of prerequisite checks starts.</span></span> <span data-ttu-id="d897b-117">Dessa kontroller omfattar följande:</span><span class="sxs-lookup"><span data-stu-id="d897b-117">These checks include:</span></span>
   
   * <span data-ttu-id="d897b-118">**Domänkontrollanten hälsokontroller** tooverify både hello styrenheter är felfri och online.</span><span class="sxs-lookup"><span data-stu-id="d897b-118">**Controller health checks** tooverify that both hello device controllers are healthy and online.</span></span>
   * <span data-ttu-id="d897b-119">**Maskinvara komponenten hälsokontroller** tooverify alla hello maskinvarukomponenter på StorSimple-enheten är felfria.</span><span class="sxs-lookup"><span data-stu-id="d897b-119">**Hardware component health checks** tooverify that all hello hardware components on your StorSimple device are healthy.</span></span>
   * <span data-ttu-id="d897b-120">**DATA 0 kontrollerar** tooverify att DATA 0 är aktiverad på enheten.</span><span class="sxs-lookup"><span data-stu-id="d897b-120">**DATA 0 checks** tooverify that DATA 0 is enabled on your device.</span></span> <span data-ttu-id="d897b-121">Om inte det här gränssnittet är aktiverat måste du aktivera det och sedan försöka igen.</span><span class="sxs-lookup"><span data-stu-id="d897b-121">If this interface is not enabled, you must enable it and then retry.</span></span>

    <span data-ttu-id="d897b-122">hello uppdatering hämtas och installeras bara om alla hello kontroller har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d897b-122">hello update is downloaded and installed only if all hello checks are successfully completed.</span></span> <span data-ttu-id="d897b-123">Du meddelas när hello kontrollerar pågår.</span><span class="sxs-lookup"><span data-stu-id="d897b-123">You are notified when hello checks are in progress.</span></span> <span data-ttu-id="d897b-124">Om hello prechecks misslyckas sedan anges du med hello orsakerna till felet.</span><span class="sxs-lookup"><span data-stu-id="d897b-124">If hello prechecks fail, then you will be provided with hello reasons for failure.</span></span> <span data-ttu-id="d897b-125">Åtgärda problemen och försök sedan hello igen.</span><span class="sxs-lookup"><span data-stu-id="d897b-125">Address those issues and then retry hello operation.</span></span> <span data-ttu-id="d897b-126">Du kanske måste toocontact Microsoft-supporten om du kan lösa dessa problem själv.</span><span class="sxs-lookup"><span data-stu-id="d897b-126">You may need toocontact Microsoft Support if you cannot address these issues by yourself.</span></span>

7. <span data-ttu-id="d897b-127">När hello prechecks har slutförts, skapas ett uppdateringsjobb.</span><span class="sxs-lookup"><span data-stu-id="d897b-127">After hello prechecks are successfully completed, an update job is created.</span></span> <span data-ttu-id="d897b-128">Du meddelas när hello Uppdateringsjobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="d897b-128">You are notified when hello update job is successfully created.</span></span>
   
    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    <span data-ttu-id="d897b-130">hello uppdateringen tillämpas sedan på enheten.</span><span class="sxs-lookup"><span data-stu-id="d897b-130">hello update is then applied on your device.</span></span>

9. <span data-ttu-id="d897b-131">Det tar några timmar toocomplete hello uppdatering.</span><span class="sxs-lookup"><span data-stu-id="d897b-131">hello update takes a few hours toocomplete.</span></span> <span data-ttu-id="d897b-132">Välj hello Uppdateringsjobbet och klicka på **information** tooview hello informationen om hello jobb när som helst.</span><span class="sxs-lookup"><span data-stu-id="d897b-132">Select hello update job and click **Details** tooview hello details of hello job at any time.</span></span>

    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update8.png)

     <span data-ttu-id="d897b-134">Du kan också övervaka hello hello Uppdateringsjobbet från **Enhetsinställningar > jobb**.</span><span class="sxs-lookup"><span data-stu-id="d897b-134">You can also monitor hello progress of hello update job from **Device settings > Jobs**.</span></span> <span data-ttu-id="d897b-135">På hello **jobb** bladet kan du se hello uppdatera status.</span><span class="sxs-lookup"><span data-stu-id="d897b-135">On hello **Jobs** blade, you can see hello update progress.</span></span>

     ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. <span data-ttu-id="d897b-137">När hello jobbet har slutförts, går toohello **Enhetsinställningar > enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="d897b-137">After hello job is complete, navigate toohello **Device settings > Device updates**.</span></span> <span data-ttu-id="d897b-138">hello programvaruversionen ska nu uppdateras.</span><span class="sxs-lookup"><span data-stu-id="d897b-138">hello software version should now be updated.</span></span>

    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update4-via-portal/update9.png)

