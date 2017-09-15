


## <a name="attach-an-empty-disk"></a><span data-ttu-id="74e53-101">Ansluta en tom disk</span><span class="sxs-lookup"><span data-stu-id="74e53-101">Attach an empty disk</span></span>
<span data-ttu-id="74e53-102">Bifoga en tom disk är ett enkelt sätt att lägga till en datadisk eftersom Azure skapas VHD-fil och lagrar den i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="74e53-102">Attaching an empty disk is a simple way to add a data disk, because Azure creates the .vhd file for you and stores it in the storage account.</span></span>

1. <span data-ttu-id="74e53-103">Klicka på **virtuella datorer (klassisk)**, och väljer sedan lämplig VM.</span><span class="sxs-lookup"><span data-stu-id="74e53-103">Click **Virtual Machines (classic)**, and then select the appropriate VM.</span></span>

2. <span data-ttu-id="74e53-104">Klicka på menyn inställningar **diskar**.</span><span class="sxs-lookup"><span data-stu-id="74e53-104">In the Settings menu, click **Disks**.</span></span>

   ![Koppla en ny tom disk](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. <span data-ttu-id="74e53-106">Klicka på kommandofältet **bifoga nya**.</span><span class="sxs-lookup"><span data-stu-id="74e53-106">On the command bar, click **Attach new**.</span></span>  
    <span data-ttu-id="74e53-107">Den **bifoga den nya disken** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="74e53-107">The **Attach new disk** dialog box appears.</span></span>

    ![Koppla en ny disk](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    <span data-ttu-id="74e53-109">Fyll i följande information:</span><span class="sxs-lookup"><span data-stu-id="74e53-109">Fill in the following information:</span></span>
    - <span data-ttu-id="74e53-110">I **filnamn**, acceptera standardnamnet eller ange ett annat namn för VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="74e53-110">In **File Name**, accept the default name or type another one for the .vhd file.</span></span> <span data-ttu-id="74e53-111">Datadisken använder ett automatiskt genererat namn, även om du anger ett annat namn för VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="74e53-111">The data disk uses an automatically generated name, even if you type another name for the .vhd file.</span></span>
    - <span data-ttu-id="74e53-112">Välj den **typen** av datadisk.</span><span class="sxs-lookup"><span data-stu-id="74e53-112">Select the **Type** of the data disk.</span></span> <span data-ttu-id="74e53-113">Alla virtuella datorer stöder standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="74e53-113">All virtual machines support standard disks.</span></span> <span data-ttu-id="74e53-114">Många virtuella datorer har också stöd för premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="74e53-114">Many virtual machines also support premium disks.</span></span>
    - <span data-ttu-id="74e53-115">Välj den **storlek (GB)** av datadisk.</span><span class="sxs-lookup"><span data-stu-id="74e53-115">Select the **Size (GB)** of the data disk.</span></span>
    - <span data-ttu-id="74e53-116">För **Värdcachelagring**, väljer du ingen eller skrivskyddad.</span><span class="sxs-lookup"><span data-stu-id="74e53-116">For **Host caching**, choose none or Read Only.</span></span>
    - <span data-ttu-id="74e53-117">Klicka på OK om du vill avsluta.</span><span class="sxs-lookup"><span data-stu-id="74e53-117">Click OK to finish.</span></span>

4. <span data-ttu-id="74e53-118">När datadisken skapas och bifogas visas den i avsnittet diskar i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="74e53-118">After the data disk is created and attached, it's listed in the disks section of the VM.</span></span>

   ![Nya och tom disk har kopplats](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> <span data-ttu-id="74e53-120">När du lägger till en datadisk, måste du logga in på den virtuella datorn och initiera disken så att den kan användas.</span><span class="sxs-lookup"><span data-stu-id="74e53-120">After you add a data disk, you need to log on to the VM and initialize the disk so that it can be used.</span></span>

## <a name="how-to-attach-an-existing-disk"></a><span data-ttu-id="74e53-121">Så här: bifoga en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="74e53-121">How to: Attach an existing disk</span></span>
<span data-ttu-id="74e53-122">För att kunna ansluta en befintlig disk måste du ha en VHD-fil tillgänglig i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="74e53-122">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span> <span data-ttu-id="74e53-123">Använd den [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) för att överföra VHD-filen till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="74e53-123">Use the [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet to upload the .vhd file to the storage account.</span></span> <span data-ttu-id="74e53-124">När du har skapat och överföra VHD-filen, kan du koppla den till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="74e53-124">After you've created and uploaded the .vhd file, you can attach it to a VM.</span></span>

1. <span data-ttu-id="74e53-125">Klicka på **virtuella datorer (klassisk)**, och väljer sedan lämplig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="74e53-125">Click **Virtual Machines (classic)**, and then select the appropriate virtual machine.</span></span>

2. <span data-ttu-id="74e53-126">Klicka på menyn inställningar **diskar**.</span><span class="sxs-lookup"><span data-stu-id="74e53-126">In the Settings menu, click **Disks**.</span></span>

3. <span data-ttu-id="74e53-127">Klicka på kommandofältet **bifoga befintliga**.</span><span class="sxs-lookup"><span data-stu-id="74e53-127">On the command bar, click **Attach existing**.</span></span>

    ![Anslut en datadisk](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. <span data-ttu-id="74e53-129">Klicka på **plats**.</span><span class="sxs-lookup"><span data-stu-id="74e53-129">Click **Location**.</span></span> <span data-ttu-id="74e53-130">Visa tillgängliga storage-konton.</span><span class="sxs-lookup"><span data-stu-id="74e53-130">The available storage accounts display.</span></span> <span data-ttu-id="74e53-131">Välj sedan ett lämpligt storage-konto från dem i listan.</span><span class="sxs-lookup"><span data-stu-id="74e53-131">Next, select an appropriate storage account from those listed.</span></span>

    ![Ange disk storage-konto](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. <span data-ttu-id="74e53-133">En **lagringskonto** innehåller en eller flera behållare som innehåller hårddiskar (VHD).</span><span class="sxs-lookup"><span data-stu-id="74e53-133">A **Storage account** holds one or more containers that contain disk drives (vhds).</span></span> <span data-ttu-id="74e53-134">Välj lämpliga behållare från dem i listan.</span><span class="sxs-lookup"><span data-stu-id="74e53-134">Select the appropriate container from those listed.</span></span>

    ![Ange behållare för virtuella datorer windows](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. <span data-ttu-id="74e53-136">Den **virtuella hårddiskar** panelen visas diskenheter som lagras i behållaren.</span><span class="sxs-lookup"><span data-stu-id="74e53-136">The **vhds** panel lists the disk drives held in the container.</span></span> <span data-ttu-id="74e53-137">Klicka på en av diskarna och klicka på Välj.</span><span class="sxs-lookup"><span data-stu-id="74e53-137">Click one of the disks, and then click Select.</span></span>

    ![Ange diskavbildning för virtuella datorer windows](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. <span data-ttu-id="74e53-139">Den **bifoga den befintliga disken** panelen visas igen med den plats där lagringskonto, behållare och valda hårddisk (vhd) för att lägga till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="74e53-139">The **Attach existing disk** panel displays again, with the location containing the storage account, container, and selected hard disk (vhd) to add to the virtual machine.</span></span>

  <span data-ttu-id="74e53-140">Ange **Värdcachelagring** NONE eller Läs bara, klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="74e53-140">Set **Host caching** to none or Read only, then click OK.</span></span>

    ![Datadisk har kopplats](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
