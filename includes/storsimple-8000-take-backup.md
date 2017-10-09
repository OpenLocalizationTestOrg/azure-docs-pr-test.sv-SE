<!--author=alkohli last changed: 01/12/17-->

### <a name="tootake-a-backup"></a><span data-ttu-id="da0f1-101">tootake en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="da0f1-101">tootake a backup</span></span>

1. <span data-ttu-id="da0f1-102">Gå tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="da0f1-102">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="da0f1-103">Hello tabular lista över enheter, markera och klicka på enheten och klicka sedan på **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="da0f1-103">From hello tabular listing of devices, select and click your device and then click **All settings**.</span></span> <span data-ttu-id="da0f1-104">I hello **inställningar** bladet går för**Inställningar > Hantera > Säkerhetskopiera princip**.</span><span class="sxs-lookup"><span data-stu-id="da0f1-104">In hello **Settings** blade, go too**Settings > Manage > Backup policy**.</span></span>

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu1.png)

2. <span data-ttu-id="da0f1-106">I hello **säkerhetskopiera princip** bladet, klickar du på **+ Lägg till princip**.</span><span class="sxs-lookup"><span data-stu-id="da0f1-106">In hello **Backup policy** blade, click **+ Add policy**.</span></span>

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu2.png)

3. <span data-ttu-id="da0f1-108">I hello **skapa säkerhetskopieringsprincip** bladet, ange ett namn som innehåller mellan 3 och 150 tecken för din princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="da0f1-108">In hello **Create backup policy** blade, supply a name that contains between 3 and 150 characters for your backup policy.</span></span>

4. <span data-ttu-id="da0f1-109">Välj hello volymer toobe säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="da0f1-109">Select hello volumes toobe backed up.</span></span> <span data-ttu-id="da0f1-110">Om du väljer fler än en volym är volymerna grupperade tillsammans toocreate en kraschkonsekvent säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="da0f1-110">If you select more than one volume, these volumes are grouped together toocreate a crash-consistent backup.</span></span>

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu4.png)

5. <span data-ttu-id="da0f1-112">På bladet **Lägg till första schema**:</span><span class="sxs-lookup"><span data-stu-id="da0f1-112">On **Add first schedule** blade:</span></span>

    1. <span data-ttu-id="da0f1-113">Välj hello typ av säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="da0f1-113">Select hello type of backup.</span></span> <span data-ttu-id="da0f1-114">För snabbare återställningar väljer du **Lokal ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="da0f1-114">For faster restores, select **Local** snapshot.</span></span> <span data-ttu-id="da0f1-115">För dataåterhämtning väljer du ögonblicksbilden av **molndata**.</span><span class="sxs-lookup"><span data-stu-id="da0f1-115">For data resiliency, select **Cloud** snapshot.</span></span>
    2. <span data-ttu-id="da0f1-116">Ange hello frekvens för säkerhetskopiering i minuter, timmar, dagar eller veckor.</span><span class="sxs-lookup"><span data-stu-id="da0f1-116">Specify hello backup frequency in minutes, hours, days, or weeks.</span></span>
    3. <span data-ttu-id="da0f1-117">Välj en kvarhållningstid.</span><span class="sxs-lookup"><span data-stu-id="da0f1-117">Select a retention time.</span></span> <span data-ttu-id="da0f1-118">Hej kvarhållningsalternativen beror på hello frekvens för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="da0f1-118">hello retention choices depend on hello backup frequency.</span></span> <span data-ttu-id="da0f1-119">Till exempel för en daglig princip kan hello kvarhållning anges i veckor, medan kvarhållningen för en månatlig princip är i månader.</span><span class="sxs-lookup"><span data-stu-id="da0f1-119">For example, for a daily policy, hello retention can be specified in weeks, whereas retention for a monthly policy is in months.</span></span>
    4. <span data-ttu-id="da0f1-120">Välj hello och-datum för hello säkerhetskopieringsprincip.</span><span class="sxs-lookup"><span data-stu-id="da0f1-120">Select hello starting time and date for hello backup policy.</span></span>
    5. <span data-ttu-id="da0f1-121">Klicka på **OK** toocreate hello säkerhetskopieringsprincip.</span><span class="sxs-lookup"><span data-stu-id="da0f1-121">Click **OK** toocreate hello backup policy.</span></span>

        ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. <span data-ttu-id="da0f1-123">Klicka på **skapa** toostart hello säkerhetskopieringsprincip skapas.</span><span class="sxs-lookup"><span data-stu-id="da0f1-123">Click **Create** toostart hello backup policy creation.</span></span> <span data-ttu-id="da0f1-124">Du meddelas när hello säkerhetskopieringsprincip har skapats.</span><span class="sxs-lookup"><span data-stu-id="da0f1-124">You are notified when hello backup policy is successfully created.</span></span> <span data-ttu-id="da0f1-125">hello lista över principer för säkerhetskopiering har även uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="da0f1-125">hello list of backup policies is also updated.</span></span>
      
      ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      <span data-ttu-id="da0f1-127">Nu har du en säkerhetskopieringspolicy som skapar schemalagda säkerhetskopieringar av din volymdata.</span><span class="sxs-lookup"><span data-stu-id="da0f1-127">You now have a backup policy that creates scheduled backups of your volume data.</span></span>




