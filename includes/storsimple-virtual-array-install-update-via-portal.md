<!--author=alkohli last changed: 11/07/16 -->

#### <a name="to-install-updates-via-the-azure-portal"></a><span data-ttu-id="51bac-101">Installera uppdateringar via Azure Portal</span><span class="sxs-lookup"><span data-stu-id="51bac-101">To install updates via the Azure portal</span></span>

1. <span data-ttu-id="51bac-102">Gå till din StorSimple-enhetshanterare och välj **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="51bac-102">Go to your StorSimple Device Manager and select **Devices**.</span></span> <span data-ttu-id="51bac-103">Välj i listan över enheter som är anslutna till tjänsten och klicka på den enhet som du vill uppdatera.</span><span class="sxs-lookup"><span data-stu-id="51bac-103">From the list of devices connected to your service, select and click the device you want to update.</span></span> 

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate1m.png) 

2. <span data-ttu-id="51bac-105">I bladet **Inställningar** klickar du på **Enhetsuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="51bac-105">In the **Settings** blade, click **Device updates**.</span></span> 

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate2m.png)  

3. <span data-ttu-id="51bac-107">Du ser ett meddelande om programuppdateringar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="51bac-107">You see a message if the software updates are available.</span></span> <span data-ttu-id="51bac-108">Du kan också klicka på **Scan** (Sök).</span><span class="sxs-lookup"><span data-stu-id="51bac-108">To check for updates, you can also click **Scan**.</span></span>

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate3m.png)

    <span data-ttu-id="51bac-110">Du meddelas när genomsökningen startar och slutförs.</span><span class="sxs-lookup"><span data-stu-id="51bac-110">You will be notified when the scan starts and completes successfully.</span></span>

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate5m.png)

4. <span data-ttu-id="51bac-112">När uppdateringarna har genomsökts klickar du på alternativet för att **hämta uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="51bac-112">Once the updates are scanned, click **Download updates**.</span></span> 

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate6m.png)

5. <span data-ttu-id="51bac-114">På bladet **Nya uppdateringar** går du igenom informationen som kommer när uppdateringarna har hämtats, och du måste bekräfta installationen.</span><span class="sxs-lookup"><span data-stu-id="51bac-114">In the **New updates** blade, review the information that after the updates are downloaded, you need to confirm the installation.</span></span> <span data-ttu-id="51bac-115">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="51bac-115">Click **OK**.</span></span>

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate7m.png)

6. <span data-ttu-id="51bac-117">Du meddelas när överföringen startar och slutförs.</span><span class="sxs-lookup"><span data-stu-id="51bac-117">You are notified when the upload starts and completes successfully.</span></span>

     ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate8m.png)

5. <span data-ttu-id="51bac-119">På bladet **Enhetsuppdateringar** klickar du på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="51bac-119">In the **Device updates** blade, click **Install**.</span></span>

     ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate11m.png)   

6. <span data-ttu-id="51bac-121">På bladet **Nya uppdateringar** får du en varning om att uppdateringen är störande.</span><span class="sxs-lookup"><span data-stu-id="51bac-121">In the **New updates** blade, you are warned that the update is disruptive.</span></span> <span data-ttu-id="51bac-122">En virtuell matris är en enhet med en enda nod. Enheten startas om när den har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="51bac-122">As virtual array is a single node device, the device restarts after it is updated.</span></span> <span data-ttu-id="51bac-123">Detta avbryter pågående IO.</span><span class="sxs-lookup"><span data-stu-id="51bac-123">This disrupts any IO in progress.</span></span> <span data-ttu-id="51bac-124">Klicka på **OK** för att installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="51bac-124">Click **OK** to install the updates.</span></span> 

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate12m.png) 

7. <span data-ttu-id="51bac-126">Du får ett meddelande när installationen startar.</span><span class="sxs-lookup"><span data-stu-id="51bac-126">You are notified when the install job starts.</span></span> 

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate13m.png)

8.  <span data-ttu-id="51bac-128">När installationen är klar klickar du på länken **Visa jobb** på bladet **Enhetsuppdateringar** för att övervaka installationen.</span><span class="sxs-lookup"><span data-stu-id="51bac-128">After the install job completes successfully, click **View Job** link in the **Device updates** blade to monitor the installation.</span></span> 

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate15m.png)

    <span data-ttu-id="51bac-130">Du kommer då till bladet **Installera uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="51bac-130">This takes you to the **Install Updates** blade.</span></span> <span data-ttu-id="51bac-131">Du kan visa detaljerad information om jobbet här.</span><span class="sxs-lookup"><span data-stu-id="51bac-131">You can view detailed information about the job here.</span></span>

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate16m.png)

9. <span data-ttu-id="51bac-133">När uppdateringarna har installerats, visas ett meddelande detta i bladet enheten uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="51bac-133">After the updates are successfully installed, you see a message to this effect in the Device updates blade.</span></span> <span data-ttu-id="51bac-134">Programvaruversionen ändras till **10.0.10288.0**.</span><span class="sxs-lookup"><span data-stu-id="51bac-134">The software version also changes to **10.0.10288.0**.</span></span> 

    ![uppdatera enhet](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate17m.png)