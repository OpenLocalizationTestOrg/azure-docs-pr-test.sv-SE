<span data-ttu-id="3805d-101">När du inte längre behöver en datadisk som är bifogade tooa virtuell dator (VM) kan du enkelt frånkopplas.</span><span class="sxs-lookup"><span data-stu-id="3805d-101">When you no longer need a data disk that's attached tooa virtual machine (VM), you can easily detach it.</span></span> <span data-ttu-id="3805d-102">När du kopplar från en disk från hello VM hello disken är inte bort från lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="3805d-102">When you detach a disk from hello VM, hello disk is not removed it from storage.</span></span> <span data-ttu-id="3805d-103">Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.</span><span class="sxs-lookup"><span data-stu-id="3805d-103">If you want toouse hello existing data on hello disk again, you can reattach it toohello same VM, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="3805d-104">En virtuell dator i Azure använder olika typer av diskar – en operativsystemdisk, en lokal temporär disk och valfria datadiskar.</span><span class="sxs-lookup"><span data-stu-id="3805d-104">A VM in Azure uses different types of disks - an operating system disk, a local temporary disk, and optional data disks.</span></span> <span data-ttu-id="3805d-105">Mer information finns i avsnittet om [diskar och virtuella hårddiskar för virtuella datorer](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3805d-105">For details, see [About Disks and VHDs for Virtual Machines](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="3805d-106">Du kan inte koppla från en operativsystemdisk om du tar bort hello VM.</span><span class="sxs-lookup"><span data-stu-id="3805d-106">You cannot detach an operating system disk unless you also delete hello VM.</span></span>

## <a name="find-hello-disk"></a><span data-ttu-id="3805d-107">Hitta hello disk</span><span class="sxs-lookup"><span data-stu-id="3805d-107">Find hello disk</span></span>
<span data-ttu-id="3805d-108">Innan du kan koppla från en disk från en virtuell dator måste toofind ut hello LUN-nummer som är en identifierare för hello disk toobe oberoende.</span><span class="sxs-lookup"><span data-stu-id="3805d-108">Before you can detach a disk from a VM you need toofind out hello LUN number, which is an identifier for hello disk toobe detached.</span></span> <span data-ttu-id="3805d-109">toodo som gör följande:</span><span class="sxs-lookup"><span data-stu-id="3805d-109">toodo that, follow these steps:</span></span>

1. <span data-ttu-id="3805d-110">Öppna Azure CLI och [ansluta tooyour Azure-prenumeration](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="3805d-110">Open Azure CLI and [connect tooyour Azure subscription](../articles/xplat-cli-connect.md).</span></span> <span data-ttu-id="3805d-111">Kontrollera att du är i läget för Azure-tjänsthantering (`azure config mode asm`).</span><span class="sxs-lookup"><span data-stu-id="3805d-111">Make sure you are in Azure Service Management mode (`azure config mode asm`).</span></span>
2. <span data-ttu-id="3805d-112">Ta reda på vilka diskar som är anslutna tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="3805d-112">Find out which disks are attached tooyour VM.</span></span> <span data-ttu-id="3805d-113">hello följande exempel visar diskar för hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="3805d-113">hello following example lists disks for hello VM named `myVM`:</span></span>

    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="3805d-114">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3805d-114">hello output is similar toohello following example:</span></span>

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. <span data-ttu-id="3805d-115">Observera hello LUN eller hello **logiskt enhetsnummer** för hello disken som du vill toodetach.</span><span class="sxs-lookup"><span data-stu-id="3805d-115">Note hello LUN or hello **logical unit number** for hello disk that you want toodetach.</span></span>

## <a name="remove-operating-system-references-toohello-disk"></a><span data-ttu-id="3805d-116">Ta bort referenser toohello operativsystemdisk</span><span class="sxs-lookup"><span data-stu-id="3805d-116">Remove operating system references toohello disk</span></span>
<span data-ttu-id="3805d-117">Innan du kopplar från hello disk från hello Linux Gäst, bör du se till att alla partitioner på disk hello inte används.</span><span class="sxs-lookup"><span data-stu-id="3805d-117">Before detaching hello disk from hello Linux guest, you should make sure that all partitions on hello disk are not in use.</span></span> <span data-ttu-id="3805d-118">Se till att operativsystemet hello inte försöker tooremount dem efter en omstart.</span><span class="sxs-lookup"><span data-stu-id="3805d-118">Ensure that hello operating system does not attempt tooremount them after a reboot.</span></span> <span data-ttu-id="3805d-119">De här stegen Ångra hello configuration skapade du antagligen när [kopplar](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello disk.</span><span class="sxs-lookup"><span data-stu-id="3805d-119">These steps undo hello configuration you likely created when [attaching](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello disk.</span></span>

1. <span data-ttu-id="3805d-120">Använd hello `lsscsi` kommandot toodiscover hello disk identifierare.</span><span class="sxs-lookup"><span data-stu-id="3805d-120">Use hello `lsscsi` command toodiscover hello disk identifier.</span></span> <span data-ttu-id="3805d-121">`lsscsi` kan installeras med `yum install lsscsi` (Red Hat-baserade distributioner) eller med `apt-get install lsscsi` (Debian-baserade distributioner).</span><span class="sxs-lookup"><span data-stu-id="3805d-121">`lsscsi` can be installed by either `yum install lsscsi` (on Red Hat based distributions) or `apt-get install lsscsi` (on Debian based distributions).</span></span> <span data-ttu-id="3805d-122">Du kan hitta hello disk-ID som du söker efter med hello LUN-nummer.</span><span class="sxs-lookup"><span data-stu-id="3805d-122">You can find hello disk identifier you are looking for by using hello LUN number.</span></span> <span data-ttu-id="3805d-123">hello senaste antalet hello tuppel i varje rad är hello LUN.</span><span class="sxs-lookup"><span data-stu-id="3805d-123">hello last number in hello tuple in each row is hello LUN.</span></span> <span data-ttu-id="3805d-124">I följande exempel från hello `lsscsi`, LUN 0 mappar för  */dev/sdc*</span><span class="sxs-lookup"><span data-stu-id="3805d-124">In hello following example from `lsscsi`, LUN 0 maps too*/dev/sdc*</span></span>

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. <span data-ttu-id="3805d-125">Använd `fdisk -l <disk>` toodiscover hello partitioner som är associerade med hello disk toobe oberoende.</span><span class="sxs-lookup"><span data-stu-id="3805d-125">Use `fdisk -l <disk>` toodiscover hello partitions associated with hello disk toobe detached.</span></span> <span data-ttu-id="3805d-126">hello följande exempel visar hello utdata `/dev/sdc`:</span><span class="sxs-lookup"><span data-stu-id="3805d-126">hello following example shows hello output for `/dev/sdc`:</span></span>

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. <span data-ttu-id="3805d-127">Demontera varje partition för hello disk.</span><span class="sxs-lookup"><span data-stu-id="3805d-127">Unmount each partition listed for hello disk.</span></span> <span data-ttu-id="3805d-128">hello följande exempel demonterar `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="3805d-128">hello following example unmounts `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

4. <span data-ttu-id="3805d-129">Använd hello `blkid` kommandot toodiscovery hello UUID: er för alla partitioner.</span><span class="sxs-lookup"><span data-stu-id="3805d-129">Use hello `blkid` command toodiscovery hello UUIDs for all partitions.</span></span> <span data-ttu-id="3805d-130">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3805d-130">hello output is similar toohello following example:</span></span>

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. <span data-ttu-id="3805d-131">Ta bort poster i hello **/etc/fstab** fil som är associerad med hello-sökvägar till enheter eller UUID: er för alla partitioner för hello disk toobe oberoende.</span><span class="sxs-lookup"><span data-stu-id="3805d-131">Remove entries in hello **/etc/fstab** file associated with either hello device paths or UUIDs for all partitions for hello disk toobe detached.</span></span>  <span data-ttu-id="3805d-132">Poster i det här exemplet kan vara:</span><span class="sxs-lookup"><span data-stu-id="3805d-132">Entries for this example might be:</span></span>

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    <span data-ttu-id="3805d-133">eller</span><span class="sxs-lookup"><span data-stu-id="3805d-133">or</span></span>
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-hello-disk"></a><span data-ttu-id="3805d-134">Koppla bort hello disk</span><span class="sxs-lookup"><span data-stu-id="3805d-134">Detach hello disk</span></span>
<span data-ttu-id="3805d-135">När du har hittat hello LUN antalet hello disk och borttagna hello operativsystemet referenser du är klar toodetach den:</span><span class="sxs-lookup"><span data-stu-id="3805d-135">After you find hello LUN number of hello disk and removed hello operating system references, you're ready toodetach it:</span></span>

1. <span data-ttu-id="3805d-136">Koppla från hello valda disken från hello virtuell dator genom att köra kommandot hello `azure vm disk detach
   <virtual-machine-name> <LUN>`.</span><span class="sxs-lookup"><span data-stu-id="3805d-136">Detach hello selected disk from hello virtual machine by running hello command `azure vm disk detach
<virtual-machine-name> <LUN>`.</span></span> <span data-ttu-id="3805d-137">hello följande exempel tar bort LUN `0` från hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="3805d-137">hello following example detaches LUN `0` from hello VM named `myVM`:</span></span>
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. <span data-ttu-id="3805d-138">Du kan kontrollera om hello disk fick oberoende genom att köra `azure vm disk list` igen.</span><span class="sxs-lookup"><span data-stu-id="3805d-138">You can check if hello disk got detached by running `azure vm disk list` again.</span></span> <span data-ttu-id="3805d-139">följande exempel kontrollerar hello hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="3805d-139">hello following example checks hello VM named `myVM`:</span></span>
   
    ```azurecli
    azure vm disk list myVM
    ```

    <span data-ttu-id="3805d-140">hello utdata är liknande toohello följande exempel som visar hello datadisken är inte längre ansluten:</span><span class="sxs-lookup"><span data-stu-id="3805d-140">hello output is similar toohello following example, which shows hello data disk is no longer attached:</span></span>

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

<span data-ttu-id="3805d-141">hello frånkopplade disken kvar i lagring men är inte längre kopplade tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3805d-141">hello detached disk remains in storage but is no longer attached tooa virtual machine.</span></span>

