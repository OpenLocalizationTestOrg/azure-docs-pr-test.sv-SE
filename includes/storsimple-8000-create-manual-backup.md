
<!--author=alkohli last changed: 01/20/2017-->

#### <a name="to-create-a-manual-backup"></a><span data-ttu-id="c5b2f-101">Så här skapar du en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="c5b2f-101">To create a manual backup</span></span>

1. <span data-ttu-id="c5b2f-102">Gå till StorSimple Device Manager-tjänsten och klicka sedan på **Enheter**.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-102">Go to your StorSimple Device Manager service and then click **Devices**.</span></span> <span data-ttu-id="c5b2f-103">Välj din enhet i listan med enheter.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-103">From the tabular listing of devices, select your device.</span></span> <span data-ttu-id="c5b2f-104">Gå till **Inställningar > Hantera > Principer för säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-104">Go to **Settings > Manage > Backup policies**.</span></span>

2. <span data-ttu-id="c5b2f-105">Bladet **Principer för säkerhetskopiering** innehåller alla principer för säkerhetskopiering i tabellformat, inklusive principen för den volym som du vill säkerhetskopiera.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-105">The **Backup policies** blade lists all the backup policies in a tabular format, including the policy for the volume that you want to back up.</span></span> <span data-ttu-id="c5b2f-106">Välj principen som är kopplad till den volym som du vill säkerhetskopiera och högerklicka för att öppna snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-106">Select the policy associated with the volume you want to back up and right-click to invoke the context menu.</span></span> <span data-ttu-id="c5b2f-107">Välj **Säkerhetskopiera nu** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-107">From the dropdown list, select **Back up now**.</span></span>

    ![Skapa en manuell säkerhetskopia](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. <span data-ttu-id="c5b2f-109">Utför följande steg på bladet **Säkerhetskopiera nu**:</span><span class="sxs-lookup"><span data-stu-id="c5b2f-109">In the **Back up now** blade, do the following steps:</span></span>

    1. <span data-ttu-id="c5b2f-110">Välj lämplig **Typ av ögonblicksbild** i listrutan: **Lokal ögonblicksbild** eller **Ögonblicksbild av molndata**.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-110">Choose the appropriate **Snapshot type** from the dropdown list: **Local** snapshot or **Cloud** snapshot.</span></span> <span data-ttu-id="c5b2f-111">Välj lokal ögonblicksbild för snabbare säkerhetskopieringar eller återställningar, och ögonblicksbild av molndata för dataåterhämtning.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-111">Select local snapshot for fast backups or restores, and cloud snapshot for data resiliency.</span></span>

        ![Skapa en manuell säkerhetskopia](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. <span data-ttu-id="c5b2f-113">Starta ett jobb för att skapa en ögonblicksbild genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-113">Click **OK** to start a job to create a snapshot.</span></span> <span data-ttu-id="c5b2f-114">Ett meddelande visas längst upp på sidan när jobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-114">You will see a notification at the top of the page after the job is successfully created.</span></span>

        ![Skapa en manuell säkerhetskopia](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. <span data-ttu-id="c5b2f-116">Klicka på meddelandet för att övervaka jobbet.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-116">To monitor the job, click the notification.</span></span> <span data-ttu-id="c5b2f-117">När du gör det öppnas bladet **Jobb** där du kan visa jobbförloppet.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-117">This takes you to the **Jobs** blade where you can view the job progress.</span></span>


5. <span data-ttu-id="c5b2f-118">När säkerhetskopieringsjobbet är slutfört, går du till fliken **Säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-118">After the backup job is finished, go to the **Backup catalog** tab.</span></span>

6. <span data-ttu-id="c5b2f-119">Ställ in filterval till lämplig enhet, säkerhetskopieringsprincip och tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-119">Set the filter selections to the appropriate device, backup policy, and time range.</span></span> <span data-ttu-id="c5b2f-120">Säkerhetskopian borde visas i listan över säkerhetskopieringsuppsättningar som visas i katalogen.</span><span class="sxs-lookup"><span data-stu-id="c5b2f-120">The backup should appear in the list of backup sets that is displayed in the catalog.</span></span>

