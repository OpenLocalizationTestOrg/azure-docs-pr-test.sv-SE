


## <a name="attach-an-empty-disk"></a><span data-ttu-id="ae0e1-101">Ansluta en tom disk</span><span class="sxs-lookup"><span data-stu-id="ae0e1-101">Attach an empty disk</span></span>
<span data-ttu-id="ae0e1-102">Bifoga en tom disk är ett enkelt sätt tooadd en data-disken, eftersom Azure skapas hello VHD-fil och lagrar den i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-102">Attaching an empty disk is a simple way tooadd a data disk, because Azure creates hello .vhd file for you and stores it in hello storage account.</span></span>

1. <span data-ttu-id="ae0e1-103">Klicka på **virtuella datorer (klassisk)**, och sedan väljer hello lämpliga VM.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-103">Click **Virtual Machines (classic)**, and then select hello appropriate VM.</span></span>

2. <span data-ttu-id="ae0e1-104">Klicka på menyn Inställningar hello **diskar**.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-104">In hello Settings menu, click **Disks**.</span></span>

   ![Koppla en ny tom disk](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. <span data-ttu-id="ae0e1-106">Klicka på hello kommandofältet **bifoga nya**.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-106">On hello command bar, click **Attach new**.</span></span>  
    <span data-ttu-id="ae0e1-107">Hej **bifoga den nya disken** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-107">hello **Attach new disk** dialog box appears.</span></span>

    ![Koppla en ny disk](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    <span data-ttu-id="ae0e1-109">Fyll i hello följande information:</span><span class="sxs-lookup"><span data-stu-id="ae0e1-109">Fill in hello following information:</span></span>
    - <span data-ttu-id="ae0e1-110">I **filnamn**accepterar hello standardnamnet eller ange ett annat namn för hello VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-110">In **File Name**, accept hello default name or type another one for hello .vhd file.</span></span> <span data-ttu-id="ae0e1-111">Hej datadisk använder ett automatiskt genererat namn, även om du anger ett annat namn för hello VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-111">hello data disk uses an automatically generated name, even if you type another name for hello .vhd file.</span></span>
    - <span data-ttu-id="ae0e1-112">Välj hello **typen** av hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-112">Select hello **Type** of hello data disk.</span></span> <span data-ttu-id="ae0e1-113">Alla virtuella datorer stöder standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-113">All virtual machines support standard disks.</span></span> <span data-ttu-id="ae0e1-114">Många virtuella datorer har också stöd för premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-114">Many virtual machines also support premium disks.</span></span>
    - <span data-ttu-id="ae0e1-115">Välj hello **storlek (GB)** av hello datadisk.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-115">Select hello **Size (GB)** of hello data disk.</span></span>
    - <span data-ttu-id="ae0e1-116">För **Värdcachelagring**, väljer du ingen eller skrivskyddad.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-116">For **Host caching**, choose none or Read Only.</span></span>
    - <span data-ttu-id="ae0e1-117">Klicka på OK toofinish.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-117">Click OK toofinish.</span></span>

4. <span data-ttu-id="ae0e1-118">När hello datadisk skapas och bifogas, visas den under hello diskar i hello VM.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-118">After hello data disk is created and attached, it's listed in hello disks section of hello VM.</span></span>

   ![Nya och tom disk har kopplats](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> <span data-ttu-id="ae0e1-120">När du lägger till en datadisk, du behöver toolog på toohello VM och initierar hello disk så att den kan användas.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-120">After you add a data disk, you need toolog on toohello VM and initialize hello disk so that it can be used.</span></span>

## <a name="how-to-attach-an-existing-disk"></a><span data-ttu-id="ae0e1-121">Så här: bifoga en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="ae0e1-121">How to: Attach an existing disk</span></span>
<span data-ttu-id="ae0e1-122">För att kunna ansluta en befintlig disk måste du ha en VHD-fil tillgänglig i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-122">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span> <span data-ttu-id="ae0e1-123">Använd hello [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet tooupload hello VHD-filen toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-123">Use hello [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet tooupload hello .vhd file toohello storage account.</span></span> <span data-ttu-id="ae0e1-124">När du har skapat och överföra hello VHD-filen, kan du koppla den tooa VM.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-124">After you've created and uploaded hello .vhd file, you can attach it tooa VM.</span></span>

1. <span data-ttu-id="ae0e1-125">Klicka på **virtuella datorer (klassisk)**, och sedan väljer hello lämplig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-125">Click **Virtual Machines (classic)**, and then select hello appropriate virtual machine.</span></span>

2. <span data-ttu-id="ae0e1-126">Klicka på menyn Inställningar hello **diskar**.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-126">In hello Settings menu, click **Disks**.</span></span>

3. <span data-ttu-id="ae0e1-127">Klicka på hello kommandofältet **bifoga befintliga**.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-127">On hello command bar, click **Attach existing**.</span></span>

    ![Anslut en datadisk](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. <span data-ttu-id="ae0e1-129">Klicka på **plats**.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-129">Click **Location**.</span></span> <span data-ttu-id="ae0e1-130">Visa hello tillgängliga storage-konton.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-130">hello available storage accounts display.</span></span> <span data-ttu-id="ae0e1-131">Välj sedan ett lämpligt storage-konto från dem i listan.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-131">Next, select an appropriate storage account from those listed.</span></span>

    ![Ange disk storage-konto](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. <span data-ttu-id="ae0e1-133">En **lagringskonto** innehåller en eller flera behållare som innehåller hårddiskar (VHD).</span><span class="sxs-lookup"><span data-stu-id="ae0e1-133">A **Storage account** holds one or more containers that contain disk drives (vhds).</span></span> <span data-ttu-id="ae0e1-134">Välj lämplig hello-behållare från dem i listan.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-134">Select hello appropriate container from those listed.</span></span>

    ![Ange behållare för virtuella datorer windows](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. <span data-ttu-id="ae0e1-136">Hej **virtuella hårddiskar** panelen visas hello diskenheter som lagras i hello behållare.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-136">hello **vhds** panel lists hello disk drives held in hello container.</span></span> <span data-ttu-id="ae0e1-137">Klicka på en av diskarna hello och klicka på Välj.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-137">Click one of hello disks, and then click Select.</span></span>

    ![Ange diskavbildning för virtuella datorer windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. <span data-ttu-id="ae0e1-139">Hej **bifoga den befintliga disken** panelen visas igen med hello-plats som innehåller hello storage-konto, behållare och valda hårddisk (vhd) tooadd toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-139">hello **Attach existing disk** panel displays again, with hello location containing hello storage account, container, and selected hard disk (vhd) tooadd toohello virtual machine.</span></span>

  <span data-ttu-id="ae0e1-140">Ange **Värdcachelagring** toonone eller Läs bara, klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="ae0e1-140">Set **Host caching** toonone or Read only, then click OK.</span></span>

    ![Datadisk har kopplats](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
