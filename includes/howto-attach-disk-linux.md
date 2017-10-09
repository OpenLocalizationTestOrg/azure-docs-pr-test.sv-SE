
<span data-ttu-id="24a97-101">Mer information om diskar finns i [Om diskar och virtuella hårddiskar för Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="24a97-101">For more information about disks, see [About Disks and VHDs for Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a><span data-ttu-id="24a97-102">Ansluta en tom disk</span><span class="sxs-lookup"><span data-stu-id="24a97-102">Attach an empty disk</span></span>
1. <span data-ttu-id="24a97-103">Öppna Azure CLI 1.0 och [ansluta tooyour Azure-prenumeration](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="24a97-103">Open Azure CLI 1.0 and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="24a97-104">Kontrollera att du är i läget för Azure-tjänsthantering (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="24a97-104">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="24a97-105">Ange `azure vm disk attach-new` toocreate och koppla en ny disk som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="24a97-105">Enter `azure vm disk attach-new` toocreate and attach a new disk as shown in hello following example.</span></span> <span data-ttu-id="24a97-106">Ersätt *myVM* med hello namn för din virtuella Linux-datorn och ange hello hello diskens storlek i GB, vilket är *100GB* i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="24a97-106">Replace *myVM* with hello name of your Linux Virtual Machine and specify hello size of hello disk in GB, which is *100GB* in this example:</span></span>

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. <span data-ttu-id="24a97-107">När hello datadisk skapas och bifogas, visas den i hello utdata från `azure vm disk list <virtual-machine-name>` som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="24a97-107">After hello data disk is created and attached, it's listed in hello output of `azure vm disk list <virtual-machine-name>` as shown in hello following example:</span></span>
   
    ```azurecli
    azure vm disk list TestVM
    ```

    <span data-ttu-id="24a97-108">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="24a97-108">hello output is similar toohello following example:</span></span>

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a><span data-ttu-id="24a97-109">Ansluta en befintlig disk</span><span class="sxs-lookup"><span data-stu-id="24a97-109">Attach an existing disk</span></span>
<span data-ttu-id="24a97-110">För att kunna ansluta en befintlig disk måste du ha en VHD-fil tillgänglig i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="24a97-110">Attaching an existing disk requires that you have a .vhd available in a storage account.</span></span>

1. <span data-ttu-id="24a97-111">Öppna Azure CLI 1.0 och [ansluta tooyour Azure-prenumeration](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="24a97-111">Open Azure CLI 1.0 and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="24a97-112">Kontrollera att du är i läget för Azure-tjänsthantering (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="24a97-112">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="24a97-113">Kontrollera om hello VHD som du vill använda tooattach är redan överförts tooyour Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="24a97-113">Check if hello VHD you want tooattach is already uploaded tooyour Azure subscription:</span></span>
   
    ```azurecli
    azure vm disk list
    ```

    <span data-ttu-id="24a97-114">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="24a97-114">hello output is similar toohello following example:</span></span>

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. <span data-ttu-id="24a97-115">Om du inte hittar hello disk som du vill toouse, du kan ladda upp en lokal VHD tooyour prenumeration med hjälp av `azure vm disk create` eller `azure vm disk upload`.</span><span class="sxs-lookup"><span data-stu-id="24a97-115">If you don't find hello disk that you want toouse, you may upload a local VHD tooyour subscription by using `azure vm disk create` or `azure vm disk upload`.</span></span> <span data-ttu-id="24a97-116">Ett exempel på `disk create` skulle vara som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="24a97-116">An example of `disk create` would be as in hello following example:</span></span>
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    <span data-ttu-id="24a97-117">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="24a97-117">hello output is similar toohello following example:</span></span>

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   <span data-ttu-id="24a97-118">Du kan också använda `azure vm disk upload` tooupload ett VHD tooa specifika storage-konto.</span><span class="sxs-lookup"><span data-stu-id="24a97-118">You may also use `azure vm disk upload` tooupload a VHD tooa specific storage account.</span></span> <span data-ttu-id="24a97-119">Läs mer om hello kommandon toomanage datadiskar ditt virtuella Azure-datorn [över här](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="24a97-119">Read more about hello commands toomanage your Azure virtual machine data disks [over here](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

4. <span data-ttu-id="24a97-120">Nu du bifoga önskad hello VHD tooyour virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="24a97-120">Now you attach hello desired VHD tooyour virtual machine:</span></span>
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   <span data-ttu-id="24a97-121">Se till att tooreplace *myVM* med hello namnet på den virtuella datorn och *myVHD* med den önskade virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="24a97-121">Make sure tooreplace *myVM* with hello name of your virtual machine, and *myVHD* with your desired VHD.</span></span>

5. <span data-ttu-id="24a97-122">Du kan verifiera hello disk är anslutna toohello virtuell dator med `azure vm disk list <virtual-machine-name>`:</span><span class="sxs-lookup"><span data-stu-id="24a97-122">You can verify hello disk is attached toohello virtual machine with `azure vm disk list <virtual-machine-name>`:</span></span>
   
    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="24a97-123">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="24a97-123">hello output is similar toohello following example:</span></span>

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> <span data-ttu-id="24a97-124">När du lägger till en datadisk, du behöver toolog på toohello virtuella datorn och initiera hello disk så hello virtuell dator kan använda hello disk för lagring (se hello följande steg för mer information på hur toodo initierar hello disk).</span><span class="sxs-lookup"><span data-stu-id="24a97-124">After you add a data disk, you'll need toolog on toohello virtual machine and initialize hello disk so hello virtual machine can use hello disk for storage (see hello following steps for more information on how toodo initialize hello disk).</span></span>
> 
> 

