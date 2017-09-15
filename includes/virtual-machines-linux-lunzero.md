<span data-ttu-id="d2da9-101">När du lägger till diskar till en Linux-VM kan fel inträffa om en disk inte finns på LUN 0.</span><span class="sxs-lookup"><span data-stu-id="d2da9-101">When adding data disks to a Linux VM, you may encounter errors if a disk does not exist at LUN 0.</span></span> <span data-ttu-id="d2da9-102">Om du lägger till en disk manuellt med hjälp av den `azure vm disk attach-new` kommando och anger ett LUN (`--lun`) i stället för att tillåta att Azure-plattformen att avgöra lämpliga LUN, ta hand som en disk redan finns / kommer att finnas på LUN 0.</span><span class="sxs-lookup"><span data-stu-id="d2da9-102">If you are adding a disk manually using the `azure vm disk attach-new` command and you specify a LUN (`--lun`) rather than allowing the Azure platform to determine the appropriate LUN, take care that a disk already exists / will exist at LUN 0.</span></span> 

<span data-ttu-id="d2da9-103">Fundera på följande exempel visar en fragment på utdata från `lsscsi`:</span><span class="sxs-lookup"><span data-stu-id="d2da9-103">Consider the following example showing a snippet of the output from `lsscsi`:</span></span>

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

<span data-ttu-id="d2da9-104">Två diskar finns på LUN 0 och 1 för LUN (den första kolumnen i den `lsscsi` utdata information `[host:channel:target:lun]`).</span><span class="sxs-lookup"><span data-stu-id="d2da9-104">The two data disks exist at LUN 0 and LUN 1 (the first column in the `lsscsi` output details `[host:channel:target:lun]`).</span></span> <span data-ttu-id="d2da9-105">Båda diskarna måste vara accessbile från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d2da9-105">Both disks should be accessbile from within the VM.</span></span> <span data-ttu-id="d2da9-106">Om du hade angett den första disken som ska läggas till på LUN 1 och den andra disken på LUN 2 manuellt, syns inte diskarna rätt från inom den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d2da9-106">If you had manually specified the first disk to be added at LUN 1 and the second disk at LUN 2, you may not see the disks correctly from within your VM.</span></span>

> [!NOTE]
> <span data-ttu-id="d2da9-107">Azure `host` värdet är 5 i dessa exempel, men detta kan variera beroende på vilken typ av lagring som du väljer.</span><span class="sxs-lookup"><span data-stu-id="d2da9-107">The Azure `host` value is 5 in these examples, but this may vary depending on the type of storage you select.</span></span>
> 
> 

<span data-ttu-id="d2da9-108">Detta disk är inte ett problem med Azure, men hur där Linux-kärnan följer SCSI-specifikationer.</span><span class="sxs-lookup"><span data-stu-id="d2da9-108">This disk behavior is not an Azure problem, but the way in which the Linux kernel follows the SCSI specifications.</span></span> <span data-ttu-id="d2da9-109">När Linux-kärnan igenom SCSI-bussen för anslutna enheter, måste en enhet finns på LUN 0 för systemet att fortsätta söka efter ytterligare enheter.</span><span class="sxs-lookup"><span data-stu-id="d2da9-109">When the Linux kernel scans the SCSI bus for attached devices, a device must be found at LUN 0 in order for the system to continue scanning for additional devices.</span></span> <span data-ttu-id="d2da9-110">Som exempel:</span><span class="sxs-lookup"><span data-stu-id="d2da9-110">As such:</span></span>

* <span data-ttu-id="d2da9-111">Granska resultatet av `lsscsi` när du lägger till en datadisk för att kontrollera att du har en disk på LUN 0.</span><span class="sxs-lookup"><span data-stu-id="d2da9-111">Review the output of `lsscsi` after adding a data disk to verify that you have a disk at LUN 0.</span></span>
* <span data-ttu-id="d2da9-112">Om disken inte visas korrekt i den virtuella datorn, kontrollerar du att det finns en disk på LUN 0.</span><span class="sxs-lookup"><span data-stu-id="d2da9-112">If your disk does not show up correctly within your VM, verify a disk exists at LUN 0.</span></span>

