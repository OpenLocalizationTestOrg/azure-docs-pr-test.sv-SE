<span data-ttu-id="15ea0-101">Azure CLI 2.0 kan du skapa och hantera Azure-resurser på macOS, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="15ea0-101">The Azure CLI 2.0 allows you to create and manage your Azure resources on macOS, Linux, and Windows.</span></span> <span data-ttu-id="15ea0-102">Den här artikeln beskrivs några av de vanligaste kommandon för att skapa och hantera virtuella maskiner (VMs).</span><span class="sxs-lookup"><span data-stu-id="15ea0-102">This article details some of the most common commands to create and manage virtual machines (VMs).</span></span>

<span data-ttu-id="15ea0-103">Den här artikeln kräver Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="15ea0-103">This article requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="15ea0-104">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="15ea0-104">Run `az --version` to find the version.</span></span> <span data-ttu-id="15ea0-105">Om du behöver uppgradera kan du läsa [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="15ea0-105">If you need to upgrade, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="15ea0-106">Du kan också använda [moln Shell](/azure/cloud-shell/quickstart) från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="15ea0-106">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a><span data-ttu-id="15ea0-107">Grundläggande Azure Resource Manager-kommandon i Azure CLI</span><span class="sxs-lookup"><span data-stu-id="15ea0-107">Basic Azure Resource Manager commands in Azure CLI</span></span>
<span data-ttu-id="15ea0-108">Mer detaljerad hjälp med specifika kommandoradsväxlar och alternativ du kan använda alternativ och online-kommandot hjälp genom att skriva `az <command> <subcommand> --help`.</span><span class="sxs-lookup"><span data-stu-id="15ea0-108">For more detailed help with specific command line switches and options, you can use the online command help and options by typing `az <command> <subcommand> --help`.</span></span>

### <a name="create-vms"></a><span data-ttu-id="15ea0-109">Skapa VM:ar</span><span class="sxs-lookup"><span data-stu-id="15ea0-109">Create VMs</span></span>
| <span data-ttu-id="15ea0-110">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="15ea0-110">Task</span></span> | <span data-ttu-id="15ea0-111">Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="15ea0-111">Azure CLI commands</span></span> |
| --- | --- |
| <span data-ttu-id="15ea0-112">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="15ea0-112">Create a resource group</span></span> | `az group create --name myResourceGroup --location eastus` |
| <span data-ttu-id="15ea0-113">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-113">Create a Linux VM</span></span> | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| <span data-ttu-id="15ea0-114">Skapa en virtuell Windows-dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-114">Create a Windows VM</span></span> | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a><span data-ttu-id="15ea0-115">Hantera tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-115">Manage VM state</span></span>
| <span data-ttu-id="15ea0-116">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="15ea0-116">Task</span></span> | <span data-ttu-id="15ea0-117">Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="15ea0-117">Azure CLI commands</span></span> |
| --- | --- |
| <span data-ttu-id="15ea0-118">Starta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-118">Start a VM</span></span> | `az vm start --resource-group myResourceGroup --name myVM` |
| <span data-ttu-id="15ea0-119">Stoppa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-119">Stop a VM</span></span> | `az vm stop --resource-group myResourceGroup --name myVM` |
| <span data-ttu-id="15ea0-120">Frigöra en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-120">Deallocate a VM</span></span> | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| <span data-ttu-id="15ea0-121">Starta om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-121">Restart a VM</span></span> | `az vm restart --resource-group myResourceGroup --name myVM` |
| <span data-ttu-id="15ea0-122">Distribuera om en VM</span><span class="sxs-lookup"><span data-stu-id="15ea0-122">Redeploy a VM</span></span> | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| <span data-ttu-id="15ea0-123">Ta bort en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-123">Delete a VM</span></span> | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a><span data-ttu-id="15ea0-124">Hämta VM-information</span><span class="sxs-lookup"><span data-stu-id="15ea0-124">Get VM info</span></span>
| <span data-ttu-id="15ea0-125">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="15ea0-125">Task</span></span> | <span data-ttu-id="15ea0-126">Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="15ea0-126">Azure CLI commands</span></span> |
| --- | --- |
| <span data-ttu-id="15ea0-127">Lista över virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="15ea0-127">List VMs</span></span> | `az vm list` |
| <span data-ttu-id="15ea0-128">Hämta information om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-128">Get information about a VM</span></span> | `az vm show --resource-group myResourceGroup --name myVM` |
| <span data-ttu-id="15ea0-129">Få användning av virtuella datorresurser</span><span class="sxs-lookup"><span data-stu-id="15ea0-129">Get usage of VM resources</span></span> | `az vm list-usage --location eastus` |
| <span data-ttu-id="15ea0-130">Hämta alla tillgängliga storlekar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="15ea0-130">Get all available VM sizes</span></span> | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a><span data-ttu-id="15ea0-131">Diskar och bilder</span><span class="sxs-lookup"><span data-stu-id="15ea0-131">Disks and images</span></span>
| <span data-ttu-id="15ea0-132">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="15ea0-132">Task</span></span> | <span data-ttu-id="15ea0-133">Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="15ea0-133">Azure CLI commands</span></span> |
| --- | --- |
| <span data-ttu-id="15ea0-134">Lägg till en datadisk i en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-134">Add a data disk to a VM</span></span> | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new ` |
| <span data-ttu-id="15ea0-135">Ta bort en datadisk från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-135">Remove a data disk from a VM</span></span> | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| <span data-ttu-id="15ea0-136">Ändra storlek på en disk</span><span class="sxs-lookup"><span data-stu-id="15ea0-136">Resize a disk</span></span> | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| <span data-ttu-id="15ea0-137">Ögonblicksbild av en disk</span><span class="sxs-lookup"><span data-stu-id="15ea0-137">Snapshot a disk</span></span> | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| <span data-ttu-id="15ea0-138">Skapa avbildning av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="15ea0-138">Create image of a VM</span></span> | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| <span data-ttu-id="15ea0-139">Skapa virtuell dator från avbildningen</span><span class="sxs-lookup"><span data-stu-id="15ea0-139">Create VM from image</span></span> | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a><span data-ttu-id="15ea0-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15ea0-140">Next steps</span></span>
<span data-ttu-id="15ea0-141">Ytterligare exempel på CLI-kommandon finns i [skapa och hantera virtuella Linux-datorer med Azure CLI](../articles/virtual-machines/linux/tutorial-manage-vm.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="15ea0-141">For additional examples of the CLI commands, see the [Create and Manage Linux VMs with the Azure CLI](../articles/virtual-machines/linux/tutorial-manage-vm.md) tutorial.</span></span>

