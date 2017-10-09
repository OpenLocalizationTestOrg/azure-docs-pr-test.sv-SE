


<span data-ttu-id="89f79-101">En tillgänglighetsuppsättning hjälper dig att hålla dina virtuella datorer som är tillgänglig under driftstopp, som under underhåll.</span><span class="sxs-lookup"><span data-stu-id="89f79-101">An availability set helps keep your virtual machines available during downtime, such as during maintenance.</span></span> <span data-ttu-id="89f79-102">Placera två eller flera liknande konfigurerad virtuella datorer i en tillgänglighetsuppsättning skapar hello redundans behövs toomaintain tillgängligheten för hello program eller tjänster som den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="89f79-102">Placing two or more similarly configured virtual machines in an availability set creates hello redundancy needed toomaintain availability of hello applications or services that your virtual machine runs.</span></span> <span data-ttu-id="89f79-103">Mer information om hur det fungerar finns [hantera hello tillgängligheten för virtuella datorer][Manage hello availability of virtual machines].</span><span class="sxs-lookup"><span data-stu-id="89f79-103">For details about how this works, see [Manage hello availability of virtual machines][Manage hello availability of virtual machines].</span></span>

<span data-ttu-id="89f79-104">Det är en bästa praxis toouse både tillgänglighetsuppsättningar och belastningsutjämning slutpunkter toohelp Kontrollera att ditt program är alltid tillgänglig och fungerar effektivt.</span><span class="sxs-lookup"><span data-stu-id="89f79-104">It's a best practice toouse both availability sets and load-balancing endpoints toohelp ensure that your application is always available and running efficiently.</span></span> <span data-ttu-id="89f79-105">Mer information om belastningsutjämnade slutpunkter finns [belastningsutjämning för Azure infrastrukturtjänster][Load balancing for Azure infrastructure services].</span><span class="sxs-lookup"><span data-stu-id="89f79-105">For details about load-balanced endpoints, see [Load balancing for Azure infrastructure services][Load balancing for Azure infrastructure services].</span></span>

<span data-ttu-id="89f79-106">Du kan lägga till den klassiska virtuella datorer i en tillgänglighetsuppsättning med hjälp av ett av två alternativ:</span><span class="sxs-lookup"><span data-stu-id="89f79-106">You can add classic virtual machines into an availability set by using one of two options:</span></span>

* <span data-ttu-id="89f79-107">[Alternativ 1: Skapa en virtuell dator och en tillgänglighetsuppsättning på hello samma tid][Option 1: Create a virtual machine and an availability set at hello same time].</span><span class="sxs-lookup"><span data-stu-id="89f79-107">[Option 1: Create a virtual machine and an availability set at hello same time][Option 1: Create a virtual machine and an availability set at hello same time].</span></span> <span data-ttu-id="89f79-108">Lägg sedan till den nya virtuella datorer toohello när du skapar de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="89f79-108">Then, add new virtual machines toohello set when you create those virtual machines.</span></span>
* <span data-ttu-id="89f79-109">[Alternativ 2: Lägg till en befintlig virtuell dator tooan tillgänglighetsuppsättning][Option 2: Add an existing virtual machine tooan availability set].</span><span class="sxs-lookup"><span data-stu-id="89f79-109">[Option 2: Add an existing virtual machine tooan availability set][Option 2: Add an existing virtual machine tooan availability set].</span></span>

> [!NOTE]
> <span data-ttu-id="89f79-110">I klassiska modellen hello hello virtuella datorer som du vill använda tooput i samma tillgänglighetsuppsättning måste tillhöra toohello samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="89f79-110">In hello classic model, virtual machines that you want tooput in hello same availability set must belong toohello same cloud service.</span></span>
> 
> 

## <span data-ttu-id="89f79-111"><a id="createset"></a>Alternativ 1: skapa en virtuell dator och en tillgänglighetsuppsättning på hello samma tid</span><span class="sxs-lookup"><span data-stu-id="89f79-111"><a id="createset"> </a>Option 1: Create a virtual machine and an availability set at hello same time</span></span>
<span data-ttu-id="89f79-112">Du kan använda antingen hello Azure-portalen eller Azure PowerShell kommandon toodo.</span><span class="sxs-lookup"><span data-stu-id="89f79-112">You can use either hello Azure portal or Azure PowerShell commands toodo this.</span></span>

<span data-ttu-id="89f79-113">toouse hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="89f79-113">toouse hello Azure portal:</span></span>

1. <span data-ttu-id="89f79-114">Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="89f79-114">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="89f79-115">Hej hubbmenyn, klicka på **+ ny**, och klicka sedan på **virtuella**.</span><span class="sxs-lookup"><span data-stu-id="89f79-115">On hello hub menu, click **+ New**, and then click **Virtual Machine**.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. <span data-ttu-id="89f79-117">Välj hello Marketplace avbildning av virtuell dator du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="89f79-117">Select hello Marketplace virtual machine image you wish toouse.</span></span> <span data-ttu-id="89f79-118">Du kan välja toocreate en Linux- eller Windows virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="89f79-118">You can choose toocreate a Linux or Windows virtual machine.</span></span>
4. <span data-ttu-id="89f79-119">För hello valda virtuella datorn, kontrollera att hello distributionsmodell anges för**klassiska** och klicka sedan på **skapa**</span><span class="sxs-lookup"><span data-stu-id="89f79-119">For hello selected virtual machine, verify that hello deployment model is set too**Classic** and then click **Create**</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. <span data-ttu-id="89f79-121">Ange ett virtuellt datornamn, användarnamn och lösenord (för Windows-datorer) eller offentlig SSH-nyckel (för Linux-datorer).</span><span class="sxs-lookup"><span data-stu-id="89f79-121">Enter a virtual machine name, user name and password (for Windows machines) or SSH public key (for Linux machines).</span></span> 
6. <span data-ttu-id="89f79-122">Välj hello VM-storlek och klicka sedan på **Välj** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="89f79-122">Choose hello VM size and then click **Select** toocontinue.</span></span>
7. <span data-ttu-id="89f79-123">Välj **valfri konfiguration > tillgänglighetsuppsättning**, och välj hello tillgänglighetsuppsättning gärna tooadd hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="89f79-123">Choose **Optional Configuration > Availability set**, and select hello availability set you wish tooadd hello virtual machine to.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. <span data-ttu-id="89f79-125">Granska inställningarna.</span><span class="sxs-lookup"><span data-stu-id="89f79-125">Review your configuration settings.</span></span> <span data-ttu-id="89f79-126">När du är klar klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="89f79-126">When you're done, click **Create**.</span></span>
9. <span data-ttu-id="89f79-127">Medan Azure skapar den virtuella datorn, du kan följa förloppet för hello under **virtuella datorer** i hello hubbmenyn.</span><span class="sxs-lookup"><span data-stu-id="89f79-127">While Azure creates your virtual machine, you can track hello progress under **Virtual Machines** in hello hub menu.</span></span>

<span data-ttu-id="89f79-128">toouse Azure PowerShell-kommandon toocreate en virtuell Azure-dator och lägga till den tooa ny eller befintlig tillgänglighetsuppsättning, se [Använd Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="89f79-128">toouse Azure PowerShell commands toocreate an Azure virtual machine and add it tooa new or existing availability set, see [Use Azure PowerShell toocreate and preconfigure Windows-based virtual machines](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="89f79-129"><a id="addmachine"></a>Alternativ 2: Lägg till en befintlig virtuell dator tooan tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="89f79-129"><a id="addmachine"> </a>Option 2: Add an existing virtual machine tooan availability set</span></span>
<span data-ttu-id="89f79-130">I hello Azure-portalen, kan du lägga till befintliga klassiska virtuella datorer tooan befintliga tillgänglighetsuppsättning eller skapa en ny för dem.</span><span class="sxs-lookup"><span data-stu-id="89f79-130">In hello Azure portal, you can add existing classic virtual machines tooan existing availability set or create a new one for them.</span></span> <span data-ttu-id="89f79-131">(Kom ihåg att hello hello virtuella datorer i samma tillgänglighetsuppsättning måste tillhöra toohello samma molntjänst.) hello stegen är nästan hello samma.</span><span class="sxs-lookup"><span data-stu-id="89f79-131">(Keep in mind that hello virtual machines in hello same availability set must belong toohello same cloud service.) hello steps are almost hello same.</span></span> <span data-ttu-id="89f79-132">Du kan använda Azure PowerShell för att lägga till hello virtuella tooan befintlig tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="89f79-132">With Azure PowerShell, you can add hello virtual machine tooan existing availability set.</span></span>

1. <span data-ttu-id="89f79-133">Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="89f79-133">If you have not already done so, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="89f79-134">Hej hubbmenyn, klicka på **virtuella datorer (klassisk)**.</span><span class="sxs-lookup"><span data-stu-id="89f79-134">On hello Hub menu, click **Virtual Machines (classic)**.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. <span data-ttu-id="89f79-136">Hello lista över virtuella datorer, Välj hello namnet på hello virtuella dator som du vill tooadd toohello set.</span><span class="sxs-lookup"><span data-stu-id="89f79-136">From hello list of virtual machines, select hello name of hello virtual machine that you want tooadd toohello set.</span></span>
4. <span data-ttu-id="89f79-137">Välj **tillgänglighetsuppsättning** från hello virtuella **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="89f79-137">Choose **Availability set** from hello virtual machine **Settings**.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. <span data-ttu-id="89f79-139">Välj hello tillgänglighetsuppsättning gärna tooadd hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="89f79-139">Select hello availability set you wish tooadd hello virtual machine to.</span></span> <span data-ttu-id="89f79-140">hello virtuell dator måste tillhöra toohello samma molntjänst som hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="89f79-140">hello virtual machine must belong toohello same cloud service as hello availability set.</span></span>
   
    ![ALT bildtext](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. <span data-ttu-id="89f79-142">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="89f79-142">Click **Save**.</span></span>

<span data-ttu-id="89f79-143">toouse Azure PowerShell-kommandon, öppna en administratörsnivå Azure PowerShell-session och kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="89f79-143">toouse Azure PowerShell commands, open an administrator-level Azure PowerShell session and run hello following command.</span></span> <span data-ttu-id="89f79-144">Hello platshållare (exempelvis &lt;VmCloudServiceName&gt;), Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.</span><span class="sxs-lookup"><span data-stu-id="89f79-144">For hello placeholders (such as &lt;VmCloudServiceName&gt;), replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> <span data-ttu-id="89f79-145">hello virtuella datorn kanske startas om toobe toofinish lägger till den toohello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="89f79-145">hello virtual machine might have toobe restarted toofinish adding it toohello availability set.</span></span>
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at hello same time]: #createset
[Option 2: Add an existing virtual machine tooan availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage hello availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

