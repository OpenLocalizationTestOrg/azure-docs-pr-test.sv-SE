<span data-ttu-id="36220-101">Nu finns stöd för två felsökningsfunktioner i Azure: konsolutdata och skärmbild för Azure Virtual Machines Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="36220-101">Support for two debugging features is now available in Azure: Console Output and Screenshot support for Azure Virtual Machines Resource Manager deployment model.</span></span> 

<span data-ttu-id="36220-102">När du använder en egen avbildning i Azure eller till och med startar en av plattformsavbildningarna, kan det finnas många orsaker till att en virtuell dator övergår i tillståndet Ej startbar.</span><span class="sxs-lookup"><span data-stu-id="36220-102">When bringing your own image to Azure or even booting one of the platform images, there can be many reasons why a Virtual Machine gets into a non-bootable state.</span></span> <span data-ttu-id="36220-103">Med de här funktionerna kan du enkelt diagnostisera och återställa virtuella datorer vid startfel.</span><span class="sxs-lookup"><span data-stu-id="36220-103">These features enable you to easily diagnose and recover your Virtual Machines from boot failures.</span></span>

<span data-ttu-id="36220-104">För virtuella Linux-datorer kan du lätt visa utdata från konsolloggen på portalen:</span><span class="sxs-lookup"><span data-stu-id="36220-104">For Linux Virtual Machines, you can easily view the output of your console log from the Portal:</span></span>

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
<span data-ttu-id="36220-106">För både virtuella Windows- och Linux-datorer kan du dock även se en skärmbild av den virtuella datorn från hypervisor-program:</span><span class="sxs-lookup"><span data-stu-id="36220-106">However, for both Windows and Linux Virtual Machines, Azure also enables you to see a screenshot of the VM from the hypervisor:</span></span>

![Fel](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

<span data-ttu-id="36220-108">De här båda funktionerna finns för Azure Virtual Machines i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="36220-108">Both of these features are supported for Azure Virtual Machines in all regions.</span></span> <span data-ttu-id="36220-109">Tänk på att det kan ta upp till 10 minuter innan skärmbilder och utdata visas på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="36220-109">Note, screenshots, and output can take up to 10 minutes to appear in your storage account.</span></span>

## <a name="common-boot-errors"></a><span data-ttu-id="36220-110">Vanliga startfel</span><span class="sxs-lookup"><span data-stu-id="36220-110">Common boot errors</span></span>

- [<span data-ttu-id="36220-111">0xC000000E</span><span class="sxs-lookup"><span data-stu-id="36220-111">0xC000000E</span></span>](https://support.microsoft.com/help/4010129)
- [<span data-ttu-id="36220-112">0xC000000F</span><span class="sxs-lookup"><span data-stu-id="36220-112">0xC000000F</span></span>](https://support.microsoft.com/help/4010130)
- [<span data-ttu-id="36220-113">0xC0000011</span><span class="sxs-lookup"><span data-stu-id="36220-113">0xC0000011</span></span>](https://support.microsoft.com/help/4010134)
- [<span data-ttu-id="36220-114">0xC0000034</span><span class="sxs-lookup"><span data-stu-id="36220-114">0xC0000034</span></span>](https://support.microsoft.com/help/4010140)
- [<span data-ttu-id="36220-115">0xC0000098</span><span class="sxs-lookup"><span data-stu-id="36220-115">0xC0000098</span></span>](https://support.microsoft.com/help/4010137)
- [<span data-ttu-id="36220-116">0xC00000BA</span><span class="sxs-lookup"><span data-stu-id="36220-116">0xC00000BA</span></span>](https://support.microsoft.com/help/4010136)
- [<span data-ttu-id="36220-117">0xC000014C</span><span class="sxs-lookup"><span data-stu-id="36220-117">0xC000014C</span></span>](https://support.microsoft.com/help/4010141)
- [<span data-ttu-id="36220-118">0xC0000221</span><span class="sxs-lookup"><span data-stu-id="36220-118">0xC0000221</span></span>](https://support.microsoft.com/help/4010132)
- [<span data-ttu-id="36220-119">0xC0000225</span><span class="sxs-lookup"><span data-stu-id="36220-119">0xC0000225</span></span>](https://support.microsoft.com/help/4010138)
- [<span data-ttu-id="36220-120">0xC0000359</span><span class="sxs-lookup"><span data-stu-id="36220-120">0xC0000359</span></span>](https://support.microsoft.com/help/4010135)
- [<span data-ttu-id="36220-121">0xC0000605</span><span class="sxs-lookup"><span data-stu-id="36220-121">0xC0000605</span></span>](https://support.microsoft.com/help/4010131)
- [<span data-ttu-id="36220-122">Inget operativsystem hittades</span><span class="sxs-lookup"><span data-stu-id="36220-122">An operating system wasn't found</span></span>](https://support.microsoft.com/help/4010142)
- [<span data-ttu-id="36220-123">Startfel eller INACCESSIBLE_BOOT_DEVICE</span><span class="sxs-lookup"><span data-stu-id="36220-123">Boot failure or INACCESSIBLE_BOOT_DEVICE</span></span>](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a><span data-ttu-id="36220-124">Aktivera diagnostik på en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="36220-124">Enable diagnostics on a new virtual machine</span></span>
1. <span data-ttu-id="36220-125">När du skapar en ny virtuell dator från förhandsversionsportalen, väljer du **Azure Resource Manager** från listrutan med distributionsmodeller:</span><span class="sxs-lookup"><span data-stu-id="36220-125">When creating a new Virtual Machine from the Preview Portal, select the **Azure Resource Manager** from the deployment model dropdown:</span></span>
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. <span data-ttu-id="36220-127">Konfigurera övervakningsalternativet och välj det lagringskonto där du vill placera de här diagnostikfilerna.</span><span class="sxs-lookup"><span data-stu-id="36220-127">Configure the Monitoring option to select the storage account where you would like to place these diagnostic files.</span></span>
 
    ![Skapa en virtuell dator](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. <span data-ttu-id="36220-129">Om du distribuerar från en Azure Resource Manager-mall går du till den virtuella datorresursen och lägger till diagnostikprofilavsnittet.</span><span class="sxs-lookup"><span data-stu-id="36220-129">If you are deploying from an Azure Resource Manager template, navigate to your Virtual Machine resource and append the diagnostics profile section.</span></span> <span data-ttu-id="36220-130">Kom ihåg att använda API-versionsrubriken 2015-06-15.</span><span class="sxs-lookup"><span data-stu-id="36220-130">Remember to use the “2015-06-15” API version header.</span></span>

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. <span data-ttu-id="36220-131">Via diagnostikprofilen kan du välja det lagringskonto där loggarna ska placeras.</span><span class="sxs-lookup"><span data-stu-id="36220-131">The diagnostics profile enables you to select the storage account where you want to put these logs.</span></span>

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

<span data-ttu-id="36220-132">Om du vill distribuera en virtuell exempeldator med aktiverad startdiagnostik, kan du kolla in vår repo här.</span><span class="sxs-lookup"><span data-stu-id="36220-132">To deploy a sample Virtual Machine with boot diagnostics enabled, check out our repo here.</span></span>

## <a name="update-an-existing-virtual-machine"></a><span data-ttu-id="36220-133">Uppdatera en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="36220-133">Update an existing virtual machine</span></span> ##

<span data-ttu-id="36220-134">Du kan också uppdatera en befintlig virtuell dator via portalen om du vill aktivera startdiagnostik via portalen.</span><span class="sxs-lookup"><span data-stu-id="36220-134">To enable boot diagnostics through the Portal, you can also update an existing Virtual Machine through the Portal.</span></span> <span data-ttu-id="36220-135">Välj alternativet Startdiagnostik och Spara.</span><span class="sxs-lookup"><span data-stu-id="36220-135">Select the Boot Diagnostics option and Save.</span></span> <span data-ttu-id="36220-136">Starta om den virtuella datorn så att ändringarna börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="36220-136">Restart the VM to take effect.</span></span>

![Uppdatera befintlig virtuell dator](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

