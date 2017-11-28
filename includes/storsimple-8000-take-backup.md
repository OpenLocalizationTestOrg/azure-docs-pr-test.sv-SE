<!--author=alkohli last changed: 01/12/17-->

### <a name="to-take-a-backup"></a><span data-ttu-id="d2f7a-101">Gör en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="d2f7a-101">To take a backup</span></span>

1. <span data-ttu-id="d2f7a-102">Gå till StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-102">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="d2f7a-103">Markera och klicka på din enhet i tabellistan med enheter och klicka sedan på **Alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-103">From the tabular listing of devices, select and click your device and then click **All settings**.</span></span> <span data-ttu-id="d2f7a-104">Gå till **Inställningar > Hantera > Säkerhetskopieringspolicy** på bladet **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-104">In the **Settings** blade, go to **Settings > Manage > Backup policy**.</span></span>

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu1.png)

2. <span data-ttu-id="d2f7a-106">Klicka på **+ Lägg till princip** på bladet **Säkerhetskopieringspolicy**.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-106">In the **Backup policy** blade, click **+ Add policy**.</span></span>

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu2.png)

3. <span data-ttu-id="d2f7a-108">På bladet **Skapa säkerhetskopieringspolicy** anger du ett namn som innehåller mellan 3 och 150 tecken för säkerhetskopieringspolicyn.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-108">In the **Create backup policy** blade, supply a name that contains between 3 and 150 characters for your backup policy.</span></span>

4. <span data-ttu-id="d2f7a-109">Välj de volymer som ska säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-109">Select the volumes to be backed up.</span></span> <span data-ttu-id="d2f7a-110">Om du väljer mer än en volym grupperas volymerna för att skapa en kraschkonsekvent säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-110">If you select more than one volume, these volumes are grouped together to create a crash-consistent backup.</span></span>

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu4.png)

5. <span data-ttu-id="d2f7a-112">På bladet **Lägg till första schema**:</span><span class="sxs-lookup"><span data-stu-id="d2f7a-112">On **Add first schedule** blade:</span></span>

    1. <span data-ttu-id="d2f7a-113">Välj typen av säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-113">Select the type of backup.</span></span> <span data-ttu-id="d2f7a-114">För snabbare återställningar väljer du **Lokal ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-114">For faster restores, select **Local** snapshot.</span></span> <span data-ttu-id="d2f7a-115">För dataåterhämtning väljer du ögonblicksbilden av **molndata**.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-115">For data resiliency, select **Cloud** snapshot.</span></span>
    2. <span data-ttu-id="d2f7a-116">Ange frekvensen för säkerhetskopiering i minuter, timmar, dagar eller veckor.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-116">Specify the backup frequency in minutes, hours, days, or weeks.</span></span>
    3. <span data-ttu-id="d2f7a-117">Välj en kvarhållningstid.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-117">Select a retention time.</span></span> <span data-ttu-id="d2f7a-118">Kvarhållningsalternativen beror på frekvensen för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-118">The retention choices depend on the backup frequency.</span></span> <span data-ttu-id="d2f7a-119">För en daglig princip, kan kvarhållning anges i veckor, medan kvarhållningen för en månatlig princip är i månader.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-119">For example, for a daily policy, the retention can be specified in weeks, whereas retention for a monthly policy is in months.</span></span>
    4. <span data-ttu-id="d2f7a-120">Välj starttiden och datum för principen för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-120">Select the starting time and date for the backup policy.</span></span>
    5. <span data-ttu-id="d2f7a-121">Skapa säkerhetskopieringspolicyn genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-121">Click **OK** to create the backup policy.</span></span>

        ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. <span data-ttu-id="d2f7a-123">Starta genereringen av säkerhetskopieringspolicyn genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-123">Click **Create** to start the backup policy creation.</span></span> <span data-ttu-id="d2f7a-124">Du får ett meddelande när säkerhetskopieringspolicyn har skapats.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-124">You are notified when the backup policy is successfully created.</span></span> <span data-ttu-id="d2f7a-125">Listan över säkerhetskopieringspolicyer uppdateras också.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-125">The list of backup policies is also updated.</span></span>
      
      ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      <span data-ttu-id="d2f7a-127">Nu har du en säkerhetskopieringspolicy som skapar schemalagda säkerhetskopieringar av din volymdata.</span><span class="sxs-lookup"><span data-stu-id="d2f7a-127">You now have a backup policy that creates scheduled backups of your volume data.</span></span>




