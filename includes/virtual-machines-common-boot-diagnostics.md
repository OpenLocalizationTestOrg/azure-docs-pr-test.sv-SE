<span data-ttu-id="b9894-101">Nu finns stöd för två felsökningsfunktioner i Azure: konsolutdata och skärmbild för Azure Virtual Machines Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b9894-101">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="b9894-102">När en egen avbildning tooAzure eller även starta en av hello plattform bilder, kan det finnas många orsaker till varför en virtuell dator hämtar till tillståndet ej startbar.</span><span class="sxs-lookup"><span data-stu-id="b9894-102">When bringing your own image tooAzure or even booting one of hello platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="b9894-103">Dessa funktioner aktivera du tooeasily diagnostisera och återställa dina virtuella datorer från startfel.</span><span class="sxs-lookup"><span data-stu-id="b9894-103">These features enable you tooeasily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="b9894-104">För Linux virtuella datorer kan du lätt visa hello utdata från konsolen loggen från hello Portal:</span><span class="sxs-lookup"><span data-stu-id="b9894-104">For Linux Virtual Machines, you can easily view hello output of your console log from hello Portal:</span></span>

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="b9894-106">Men för både Windows och Linux virtuella datorer kan Azure också toosee en skärmbild av hello VM från hello hypervisor-programmet:</span><span class="sxs-lookup"><span data-stu-id="b9894-106">However, for both Windows and Linux Virtual Machines, Azure also enables you toosee a screenshot of hello VM from hello hypervisor:</span></span>

![Fel](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

<span data-ttu-id="b9894-108">De här båda funktionerna finns för Azure Virtual Machines i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="b9894-108">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="b9894-109">Obs skärmbilder och utdata kan ta upp too10 minuter tooappear i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b9894-109">Note, screenshots, and output can take up too10 minutes tooappear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="b9894-110">Vanliga startfel</span><span class="sxs-lookup"><span data-stu-id="b9894-110">Common boot errors</span></span>

- [<span data-ttu-id="b9894-111">0xC000000E</span><span class="sxs-lookup"><span data-stu-id="b9894-111">0xC000000E</span></span>](https://support.microsoft.com/help/4010129)
- [<span data-ttu-id="b9894-112">0xC000000F</span><span class="sxs-lookup"><span data-stu-id="b9894-112">0xC000000F</span></span>](https://support.microsoft.com/help/4010130)
- [<span data-ttu-id="b9894-113">0xC0000011</span><span class="sxs-lookup"><span data-stu-id="b9894-113">0xC0000011</span></span>](https://support.microsoft.com/help/4010134)
- [<span data-ttu-id="b9894-114">0xC0000034</span><span class="sxs-lookup"><span data-stu-id="b9894-114">0xC0000034</span></span>](https://support.microsoft.com/help/4010140)
- [<span data-ttu-id="b9894-115">0xC0000098</span><span class="sxs-lookup"><span data-stu-id="b9894-115">0xC0000098</span></span>](https://support.microsoft.com/help/4010137)
- [<span data-ttu-id="b9894-116">0xC00000BA</span><span class="sxs-lookup"><span data-stu-id="b9894-116">0xC00000BA</span></span>](https://support.microsoft.com/help/4010136)
- [<span data-ttu-id="b9894-117">0xC000014C</span><span class="sxs-lookup"><span data-stu-id="b9894-117">0xC000014C</span></span>](https://support.microsoft.com/help/4010141)
- [<span data-ttu-id="b9894-118">0xC0000221</span><span class="sxs-lookup"><span data-stu-id="b9894-118">0xC0000221</span></span>](https://support.microsoft.com/help/4010132)
- [<span data-ttu-id="b9894-119">0xC0000225</span><span class="sxs-lookup"><span data-stu-id="b9894-119">0xC0000225</span></span>](https://support.microsoft.com/help/4010138)
- [<span data-ttu-id="b9894-120">0xC0000359</span><span class="sxs-lookup"><span data-stu-id="b9894-120">0xC0000359</span></span>](https://support.microsoft.com/help/4010135)
- [<span data-ttu-id="b9894-121">0xC0000605</span><span class="sxs-lookup"><span data-stu-id="b9894-121">0xC0000605</span></span>](https://support.microsoft.com/help/4010131)
- [<span data-ttu-id="b9894-122">Inget operativsystem hittades</span><span class="sxs-lookup"><span data-stu-id="b9894-122">An operating system wasn't found</span></span>](https://support.microsoft.com/help/4010142)
- [<span data-ttu-id="b9894-123">Startfel eller INACCESSIBLE_BOOT_DEVICE</span><span class="sxs-lookup"><span data-stu-id="b9894-123">Boot failure or INACCESSIBLE_BOOT_DEVICE</span></span>](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="b9894-124">Aktivera diagnostik på en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b9894-124">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="b9894-125">När du skapar en ny virtuell dator från hello Preview Portal väljer hello **Azure Resource Manager** hello distribution modellen listrutan:</span><span class="sxs-lookup"><span data-stu-id="b9894-125">When creating a new Virtual Machine from hello Preview Portal, select hello **Azure Resource Manager** from hello deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="b9894-127">Konfigurera hello övervakning alternativet tooselect hello storage-konto där du vill ha tooplace filerna diagnostik.</span><span class="sxs-lookup"><span data-stu-id="b9894-127">Configure hello Monitoring option tooselect hello storage account where you would like tooplace these diagnostic files.</span></span>
 
    ![Skapa en virtuell dator](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="b9894-129">Om du distribuerar en Azure Resource Manager-mall, navigera tooyour virtuella datorresurser och Lägg till hello diagnostik profil avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b9894-129">If you are deploying from an Azure Resource Manager template, navigate tooyour Virtual Machine resource and append hello diagnostics profile section.</span></span> <span data-ttu-id="b9894-130">Kom ihåg toouse hello ”2015-06-15” API version-huvud.</span><span class="sxs-lookup"><span data-stu-id="b9894-130">Remember toouse hello “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="b9894-131">Hej diagnostikprofilen kan tooselect hello storage-konto där du vill att tooput dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="b9894-131">hello diagnostics profile enables you tooselect hello storage account where you want tooput these logs.</span></span>

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

<span data-ttu-id="b9894-132">toodeploy ett exempel på en virtuell dator med startdiagnostikinställningar aktiverad checka ut våra lagringsplatsen här.</span><span class="sxs-lookup"><span data-stu-id="b9894-132">toodeploy a sample Virtual Machine with boot diagnostics enabled, check out our repo here.</span></span>

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="b9894-133">Uppdatera en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b9894-133">Update an existing virtual machine</span></span> ##

<span data-ttu-id="b9894-134">tooenable startdiagnostikinställningar via hello Portal, kan du även uppdatera en befintlig virtuell dator via hello Portal.</span><span class="sxs-lookup"><span data-stu-id="b9894-134">tooenable boot diagnostics through hello Portal, you can also update an existing Virtual Machine through hello Portal.</span></span> <span data-ttu-id="b9894-135">Välj hello Startdiagnostikinställningar alternativet och spara.</span><span class="sxs-lookup"><span data-stu-id="b9894-135">Select hello Boot Diagnostics option and Save.</span></span> <span data-ttu-id="b9894-136">Starta om hello VM tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="b9894-136">Restart hello VM tootake effect.</span></span>

![Uppdatera befintlig virtuell dator](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

