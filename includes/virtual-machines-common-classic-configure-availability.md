


<span data-ttu-id="de769-101">En tillgänglighetsuppsättning hjälper dig att hålla dina virtuella datorer som är tillgänglig under driftstopp, som under underhåll.</span><span class="sxs-lookup"><span data-stu-id="de769-101">An availability set helps keep your virtual machines available during downtime, such as during maintenance.</span></span> <span data-ttu-id="de769-102">Placera två eller flera liknande konfigurerad virtuella datorer i en tillgänglighetsuppsättning skapar redundans krävs för att bibehålla tillgängligheten för program eller tjänster som den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="de769-102">Placing two or more similarly configured virtual machines in an availability set creates the redundancy needed to maintain availability of the applications or services that your virtual machine runs.</span></span> <span data-ttu-id="de769-103">Mer information om hur det fungerar finns [hantera tillgängligheten för virtuella datorer][Manage the availability of virtual machines].</span><span class="sxs-lookup"><span data-stu-id="de769-103">For details about how this works, see [Manage the availability of virtual machines][Manage the availability of virtual machines].</span></span>

<span data-ttu-id="de769-104">Det är bäst att använda både tillgänglighetsuppsättningar och belastningsutjämning slutpunkter för att säkerställa att programmet alltid är tillgängliga och fungerar effektivt.</span><span class="sxs-lookup"><span data-stu-id="de769-104">It's a best practice to use both availability sets and load-balancing endpoints to help ensure that your application is always available and running efficiently.</span></span> <span data-ttu-id="de769-105">Mer information om belastningsutjämnade slutpunkter finns [belastningsutjämning för Azure infrastrukturtjänster][Load balancing for Azure infrastructure services].</span><span class="sxs-lookup"><span data-stu-id="de769-105">For details about load-balanced endpoints, see [Load balancing for Azure infrastructure services][Load balancing for Azure infrastructure services].</span></span>

<span data-ttu-id="de769-106">Du kan lägga till den klassiska virtuella datorer i en tillgänglighetsuppsättning med hjälp av ett av två alternativ:</span><span class="sxs-lookup"><span data-stu-id="de769-106">You can add classic virtual machines into an availability set by using one of two options:</span></span>

* <span data-ttu-id="de769-107">[Alternativ 1: Skapa en virtuell dator och en tillgänglighetsuppsättning samtidigt][Option 1: Create a virtual machine and an availability set at the same time].</span><span class="sxs-lookup"><span data-stu-id="de769-107">[Option 1: Create a virtual machine and an availability set at the same time][Option 1: Create a virtual machine and an availability set at the same time].</span></span> <span data-ttu-id="de769-108">Sedan kan lägga till nya virtuella datorer till uppsättningen med när du skapar de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="de769-108">Then, add new virtual machines to the set when you create those virtual machines.</span></span>
* <span data-ttu-id="de769-109">[Alternativ 2: Lägg till en befintlig virtuell dator i en tillgänglighetsuppsättning][Option 2: Add an existing virtual machine to an availability set].</span><span class="sxs-lookup"><span data-stu-id="de769-109">[Option 2: Add an existing virtual machine to an availability set][Option 2: Add an existing virtual machine to an availability set].</span></span>

> [!NOTE]
> <span data-ttu-id="de769-110">I den klassiska modellen, måste virtuella datorer som du vill ska ingå i samma tillgänglighetsuppsättning tillhöra samma tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="de769-110">In the classic model, virtual machines that you want to put in the same availability set must belong to the same cloud service.</span></span>
> 
> 

## <span data-ttu-id="de769-111"><a id="createset"></a>Alternativ 1: skapa en virtuell dator och en tillgänglighetsuppsättning på samma gång</span><span class="sxs-lookup"><span data-stu-id="de769-111"><a id="createset"> </a>Option 1: Create a virtual machine and an availability set at the same time</span></span>
<span data-ttu-id="de769-112">Du kan använda Azure-portalen eller Azure PowerShell-kommandon för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="de769-112">You can use either the Azure portal or Azure PowerShell commands to do this.</span></span>

<span data-ttu-id="de769-113">Använda Azure portal:</span><span class="sxs-lookup"><span data-stu-id="de769-113">To use the Azure portal:</span></span>

1. <span data-ttu-id="de769-114">Om du inte redan gjort det loggar du in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de769-114">If you haven't already done so, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="de769-115">På navmenyn klickar du på **+ ny**, och klicka sedan på **virtuella**.</span><span class="sxs-lookup"><span data-stu-id="de769-115">On the hub menu, click **+ New**, and then click **Virtual Machine**.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. <span data-ttu-id="de769-117">Välj Marketplace-virtuell-avbildning som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="de769-117">Select the Marketplace virtual machine image you wish to use.</span></span> <span data-ttu-id="de769-118">Du kan välja att skapa en Linux- eller Windows virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="de769-118">You can choose to create a Linux or Windows virtual machine.</span></span>
4. <span data-ttu-id="de769-119">För den valda virtuella datorn kontrollerar du att distributionsmodell är inställd på **klassiska** och klicka sedan på **skapa**</span><span class="sxs-lookup"><span data-stu-id="de769-119">For the selected virtual machine, verify that the deployment model is set to **Classic** and then click **Create**</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. <span data-ttu-id="de769-121">Ange ett virtuellt datornamn, användarnamn och lösenord (för Windows-datorer) eller offentlig SSH-nyckel (för Linux-datorer).</span><span class="sxs-lookup"><span data-stu-id="de769-121">Enter a virtual machine name, user name and password (for Windows machines) or SSH public key (for Linux machines).</span></span> 
6. <span data-ttu-id="de769-122">Välj VM-storlek och klicka sedan på **Välj** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="de769-122">Choose the VM size and then click **Select** to continue.</span></span>
7. <span data-ttu-id="de769-123">Välj **valfri konfiguration > tillgänglighetsuppsättning**, och välj tillgänglighetsuppsättning du vill lägga till den virtuella datorn till.</span><span class="sxs-lookup"><span data-stu-id="de769-123">Choose **Optional Configuration > Availability set**, and select the availability set you wish to add the virtual machine to.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. <span data-ttu-id="de769-125">Granska inställningarna.</span><span class="sxs-lookup"><span data-stu-id="de769-125">Review your configuration settings.</span></span> <span data-ttu-id="de769-126">När du är klar klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="de769-126">When you're done, click **Create**.</span></span>
9. <span data-ttu-id="de769-127">Medan Azure skapar den virtuella datorn, du kan följa förloppet under **virtuella datorer** på navmenyn.</span><span class="sxs-lookup"><span data-stu-id="de769-127">While Azure creates your virtual machine, you can track the progress under **Virtual Machines** in the hub menu.</span></span>

<span data-ttu-id="de769-128">Om du vill använda Azure PowerShell-kommandon för att skapa en virtuell Azure-dator och lägga till den i en ny eller befintlig tillgänglighetsuppsättning, se [använda Azure PowerShell för att skapa och förkonfigurera Windows-baserade virtuella datorer](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="de769-128">To use Azure PowerShell commands to create an Azure virtual machine and add it to a new or existing availability set, see [Use Azure PowerShell to create and preconfigure Windows-based virtual machines](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="de769-129"><a id="addmachine"></a>Alternativ 2: Lägg till en befintlig virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="de769-129"><a id="addmachine"> </a>Option 2: Add an existing virtual machine to an availability set</span></span>
<span data-ttu-id="de769-130">Du kan lägga till befintliga klassiska virtuella datorer i en befintlig tillgänglighetsuppsättning ange eller skapa en ny för dem i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="de769-130">In the Azure portal, you can add existing classic virtual machines to an existing availability set or create a new one for them.</span></span> <span data-ttu-id="de769-131">(Tänk på att de virtuella datorerna i samma tillgänglighetsuppsättning måste tillhöra samma Molntjänsten.) Stegen är nästan desamma.</span><span class="sxs-lookup"><span data-stu-id="de769-131">(Keep in mind that the virtual machines in the same availability set must belong to the same cloud service.) The steps are almost the same.</span></span> <span data-ttu-id="de769-132">Du kan använda Azure PowerShell för att lägga till den virtuella datorn till en befintlig tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="de769-132">With Azure PowerShell, you can add the virtual machine to an existing availability set.</span></span>

1. <span data-ttu-id="de769-133">Om du inte redan har gjort det, logga in på den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de769-133">If you have not already done so, sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="de769-134">På navmenyn klickar du på **virtuella datorer (klassisk)**.</span><span class="sxs-lookup"><span data-stu-id="de769-134">On the Hub menu, click **Virtual Machines (classic)**.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. <span data-ttu-id="de769-136">Lista över virtuella datorer, Välj namnet på den virtuella dator som du vill lägga till i uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="de769-136">From the list of virtual machines, select the name of the virtual machine that you want to add to the set.</span></span>
4. <span data-ttu-id="de769-137">Välj **tillgänglighetsuppsättning** från den virtuella datorn **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="de769-137">Choose **Availability set** from the virtual machine **Settings**.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. <span data-ttu-id="de769-139">Välj den tillgänglighetsuppsättningen som du vill lägga till den virtuella datorn till.</span><span class="sxs-lookup"><span data-stu-id="de769-139">Select the availability set you wish to add the virtual machine to.</span></span> <span data-ttu-id="de769-140">Den virtuella datorn måste tillhöra samma Molntjänsten som tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="de769-140">The virtual machine must belong to the same cloud service as the availability set.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. <span data-ttu-id="de769-142">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="de769-142">Click **Save**.</span></span>

<span data-ttu-id="de769-143">Öppna en administratörsnivå Azure PowerShell-session och kör följande kommando för att använda Azure PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="de769-143">To use Azure PowerShell commands, open an administrator-level Azure PowerShell session and run the following command.</span></span> <span data-ttu-id="de769-144">Platshållare (exempelvis &lt;VmCloudServiceName&gt;), Ersätt allt inom citattecken, inklusive den < och > tecken, med rätt namn.</span><span class="sxs-lookup"><span data-stu-id="de769-144">For the placeholders (such as &lt;VmCloudServiceName&gt;), replace everything within the quotes, including the < and > characters, with the correct names.</span></span>

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> <span data-ttu-id="de769-145">Den virtuella datorn kanske måste startas om för att du har lagt till den tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="de769-145">The virtual machine might have to be restarted to finish adding it to the availability set.</span></span>
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

