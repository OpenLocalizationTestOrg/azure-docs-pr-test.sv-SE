
<!--author=alkohli last changed: 01/20/2017-->

#### <a name="toocreate-a-manual-backup"></a><span data-ttu-id="7ed78-101">toocreate en manuell säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="7ed78-101">toocreate a manual backup</span></span>

1. <span data-ttu-id="7ed78-102">Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **enheter**.</span><span class="sxs-lookup"><span data-stu-id="7ed78-102">Go tooyour StorSimple Device Manager service and then click **Devices**.</span></span> <span data-ttu-id="7ed78-103">Hello tabular lista över enheter, Välj din enhet.</span><span class="sxs-lookup"><span data-stu-id="7ed78-103">From hello tabular listing of devices, select your device.</span></span> <span data-ttu-id="7ed78-104">Gå för**Inställningar > Hantera > Säkerhetskopieringsprinciper**.</span><span class="sxs-lookup"><span data-stu-id="7ed78-104">Go too**Settings > Manage > Backup policies**.</span></span>

2. <span data-ttu-id="7ed78-105">Hej **Säkerhetskopieringsprinciper** bladet visar en lista över alla hello principer för säkerhetskopiering i tabulärt format, inklusive hello princip för hello volym som du vill att tooback.</span><span class="sxs-lookup"><span data-stu-id="7ed78-105">hello **Backup policies** blade lists all hello backup policies in a tabular format, including hello policy for hello volume that you want tooback up.</span></span> <span data-ttu-id="7ed78-106">Välj hello principen som är associerad med hello volym som du vill att tooback och högerklicka tooinvoke hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="7ed78-106">Select hello policy associated with hello volume you want tooback up and right-click tooinvoke hello context menu.</span></span> <span data-ttu-id="7ed78-107">Hello listrutan, Välj **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="7ed78-107">From hello dropdown list, select **Back up now**.</span></span>

    ![Skapa en manuell säkerhetskopia](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. <span data-ttu-id="7ed78-109">I hello **Säkerhetskopiera nu** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ed78-109">In hello **Back up now** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="7ed78-110">Välj lämplig hello **ögonblicksbilder** hello listrutan: **lokala** ögonblicksbild eller **moln** ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7ed78-110">Choose hello appropriate **Snapshot type** from hello dropdown list: **Local** snapshot or **Cloud** snapshot.</span></span> <span data-ttu-id="7ed78-111">Välj lokal ögonblicksbild för snabbare säkerhetskopieringar eller återställningar, och ögonblicksbild av molndata för dataåterhämtning.</span><span class="sxs-lookup"><span data-stu-id="7ed78-111">Select local snapshot for fast backups or restores, and cloud snapshot for data resiliency.</span></span>

        ![Skapa en manuell säkerhetskopia](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. <span data-ttu-id="7ed78-113">Klicka på **OK** toostart jobbet-toocreate en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7ed78-113">Click **OK** toostart a job toocreate a snapshot.</span></span> <span data-ttu-id="7ed78-114">Ett meddelande hello överst på hello sidan visas när hello jobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="7ed78-114">You will see a notification at hello top of hello page after hello job is successfully created.</span></span>

        ![Skapa en manuell säkerhetskopia](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. <span data-ttu-id="7ed78-116">toomonitor hello jobbet, klickar du på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="7ed78-116">toomonitor hello job, click hello notification.</span></span> <span data-ttu-id="7ed78-117">Detta tar toohello **jobb** bladet där du kan visa hello jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="7ed78-117">This takes you toohello **Jobs** blade where you can view hello job progress.</span></span>


5. <span data-ttu-id="7ed78-118">När hello säkerhetskopieringsjobbet är slutfört, går toohello **säkerhetskopieringskatalog** fliken.</span><span class="sxs-lookup"><span data-stu-id="7ed78-118">After hello backup job is finished, go toohello **Backup catalog** tab.</span></span>

6. <span data-ttu-id="7ed78-119">Ange hello filter val toohello lämplig enhet, säkerhetskopieringsprincip och tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="7ed78-119">Set hello filter selections toohello appropriate device, backup policy, and time range.</span></span> <span data-ttu-id="7ed78-120">hello säkerhetskopiering ska visas i hello listan över säkerhetskopieringsuppsättningar som visas i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="7ed78-120">hello backup should appear in hello list of backup sets that is displayed in hello catalog.</span></span>

