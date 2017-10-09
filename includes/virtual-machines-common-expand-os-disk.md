## <a name="overview"></a><span data-ttu-id="ef5ae-101">Översikt</span><span class="sxs-lookup"><span data-stu-id="ef5ae-101">Overview</span></span>
<span data-ttu-id="ef5ae-102">När du skapar en ny virtuell dator (VM) i en resursgrupp genom att distribuera en avbildning från [Azure Marketplace](https://azure.microsoft.com/marketplace/), hello standardenheten OS är 127 GB.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-102">When you create a new virtual machine (VM) in a Resource Group by deploying an image from [Azure Marketplace](https://azure.microsoft.com/marketplace/), hello default OS drive is 127 GB.</span></span> <span data-ttu-id="ef5ae-103">Även om det är möjligt tooadd data diskar toohello VM (hur många beroende på hello SKU du har valt) och dessutom är det rekommenderade tooinstall program och CPU-intensiv arbetsbelastning på diskarna tillägg, måste ofta kunder tooexpand hello OS enheten toosupport vissa scenarier, till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-103">Even though it’s possible tooadd data disks toohello VM (how many depending upon hello SKU you’ve chosen) and moreover it’s recommended tooinstall applications and CPU intensive workloads on these addendum disks, oftentimes customers need tooexpand hello OS drive toosupport certain scenarios such as following:</span></span>

1. <span data-ttu-id="ef5ae-104">Stöd för äldre program som installerar komponenter på operativsystemenheten.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-104">Support legacy applications that install components on OS drive.</span></span>
2. <span data-ttu-id="ef5ae-105">Migrera en fysisk eller virtuell dator från lokal plats med större operativsystemenhet.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-105">Migrate a physical PC or virtual machine from on-premises with a larger OS drive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef5ae-106">I Azure finns två olika distributionsmodeller för att skapa och arbeta med resurser: Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-106">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="ef5ae-107">Den här artikeln täcker hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-107">This article covers using hello Resource Manager model.</span></span> <span data-ttu-id="ef5ae-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> 
> 

## <a name="resize-hello-os-drive"></a><span data-ttu-id="ef5ae-109">Ändra storlek på hello OS-enhet</span><span class="sxs-lookup"><span data-stu-id="ef5ae-109">Resize hello OS drive</span></span>
<span data-ttu-id="ef5ae-110">I den här artikeln ska vi utföra hello uppgiften för storleksändring hello OS-enhet med hjälp av resource manager moduler [Azure Powershell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="ef5ae-110">In this article we’ll accomplish hello task of resizing hello OS drive using resource manager modules of [Azure Powershell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="ef5ae-111">Öppna din Powershell ISE eller Powershell-fönster i administrativa läge och hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-111">Open your Powershell ISE or Powershell window in administrative mode and follow hello steps below:</span></span>

1. <span data-ttu-id="ef5ae-112">Logga in tooyour Microsoft Azure konto i resursen hanteringsläge och välja din prenumeration på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-112">Sign-in tooyour Microsoft Azure account in resource management mode and select your subscription as follows:</span></span>
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. <span data-ttu-id="ef5ae-113">Ange namn på resursgrupp och virtuell dator på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-113">Set your resource group name and VM name as follows:</span></span>
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. <span data-ttu-id="ef5ae-114">Hämta en referens tooyour VM på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-114">Obtain a reference tooyour VM as follows:</span></span>
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. <span data-ttu-id="ef5ae-115">Stoppa hello VM innan du ändrar storlek på hello disk på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-115">Stop hello VM before resizing hello disk as follows:</span></span>
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. <span data-ttu-id="ef5ae-116">Och här kommer hello ögonblick vi har väntat på!</span><span class="sxs-lookup"><span data-stu-id="ef5ae-116">And here comes hello moment we’ve been waiting for!</span></span> <span data-ttu-id="ef5ae-117">Ange hello storleken på hello OS toohello önskad värde och uppdatera hello VM enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-117">Set hello size of hello OS disk toohello desired value and update hello VM as follows:</span></span>
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > <span data-ttu-id="ef5ae-118">hello nya storleken måste vara större än hello befintliga diskens storlek.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-118">hello new size should be greater than hello existing disk size.</span></span> <span data-ttu-id="ef5ae-119">hello maximalt tillåtna är 1 023 GB.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-119">hello maximum allowed is 1023 GB.</span></span>
   > 
   > 
6. <span data-ttu-id="ef5ae-120">Uppdaterar hello VM kan ta några sekunder.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-120">Updating hello VM may take a few seconds.</span></span> <span data-ttu-id="ef5ae-121">När hello-kommandot har slutförts körs, startas hello VM på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-121">Once hello command finishes executing, restart hello VM as follows:</span></span>
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

<span data-ttu-id="ef5ae-122">Klart!</span><span class="sxs-lookup"><span data-stu-id="ef5ae-122">And that’s it!</span></span> <span data-ttu-id="ef5ae-123">Nu RDP till hello VM, öppna Datorhantering (eller Diskhantering) och expandera hello-enhet med hello nyligen allokerat utrymme.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-123">Now RDP into hello VM, open Computer Management (or Disk Management) and expand hello drive using hello newly allocated space.</span></span>

## <a name="summary"></a><span data-ttu-id="ef5ae-124">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ef5ae-124">Summary</span></span>
<span data-ttu-id="ef5ae-125">Vi använde Azure Resource Manager moduler Powershell tooexpand hello OS-enhet på en virtuell IaaS-dator i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-125">In this article, we used Azure Resource Manager modules of Powershell tooexpand hello OS drive of an IaaS virtual machine.</span></span> <span data-ttu-id="ef5ae-126">Nedan är hello fullständiga skriptet referens:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-126">Reproduced below is hello complete script for your reference:</span></span>

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a><span data-ttu-id="ef5ae-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef5ae-127">Next Steps</span></span>
<span data-ttu-id="ef5ae-128">Men i den här artikeln fokuserar huvudsakligen på expanderande hello VM hello OS-disk, kan hello utvecklade skript även användas för att expandera hello data diskar anslutna toohello VM genom att ändra en enda rad kod.</span><span class="sxs-lookup"><span data-stu-id="ef5ae-128">Though in this article, we focused primarily on expanding hello OS disk of hello VM, hello developed script may also be used for expanding hello data disks attached toohello VM by changing a single line of code.</span></span> <span data-ttu-id="ef5ae-129">Till exempel tooexpand hello först data disk bifogade toohello VM, Ersätt hello ```OSDisk``` objekt av ```StorageProfile``` med ```DataDisks``` matrisen och använda ett numeriskt index tooobtain referens toofirst bifogade datadisk, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-129">For example, tooexpand hello first data disk attached toohello VM, replace hello ```OSDisk``` object of ```StorageProfile``` with ```DataDisks``` array and use a numeric index tooobtain a reference toofirst attached data disk, as shown below:</span></span>

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
<span data-ttu-id="ef5ae-130">På samma sätt kan du referera till andra data diskar anslutna toohello VM, antingen med hjälp av ett index som visas ovan eller hello ```Name``` -egenskapen för hello disk som på bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="ef5ae-130">Similarly you may reference other data disks attached toohello VM, either by using an index as shown above or hello ```Name``` property of hello disk as illustrated below:</span></span>

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

<span data-ttu-id="ef5ae-131">Om du vill att toofind ut hur tooattach diskar tooan Azure Resource Manager VM, markerar du det [artikel](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef5ae-131">If you want toofind out how tooattach disks tooan Azure Resource Manager VM, check this [article](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

