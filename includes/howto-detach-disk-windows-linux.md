<span data-ttu-id="7042f-101">När du inte längre behöver en datadisk som är bifogade tooa virtuell dator kan du enkelt kan koppla från den.</span><span class="sxs-lookup"><span data-stu-id="7042f-101">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="7042f-102">Kopplar bort en disk tar bort hello disk från hello virtuell dator, men ta bort inte hello disken från hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7042f-102">Detaching a disk removes hello disk from hello virtual machine, but doesn't delete hello disk from hello Azure storage account.</span></span>

<span data-ttu-id="7042f-103">Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.</span><span class="sxs-lookup"><span data-stu-id="7042f-103">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="7042f-104">toodetach en operativsystemdisk, måste du först toodelete hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7042f-104">toodetach an operating system disk, you first need toodelete hello virtual machine.</span></span>
>

## <a name="find-hello-disk"></a><span data-ttu-id="7042f-105">Hitta hello disk</span><span class="sxs-lookup"><span data-stu-id="7042f-105">Find hello disk</span></span>
<span data-ttu-id="7042f-106">Om du inte vet hello namnet på hello disk eller vill tooverify den innan du koppla från den, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="7042f-106">If you don't know hello name of hello disk or want tooverify it before you detach it, follow these steps.</span></span>

1. <span data-ttu-id="7042f-107">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7042f-107">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="7042f-108">Klicka på **virtuella datorer**, och sedan väljer hello lämpliga VM.</span><span class="sxs-lookup"><span data-stu-id="7042f-108">Click **Virtual Machines**, and then select hello appropriate VM.</span></span>

3. <span data-ttu-id="7042f-109">Klicka på **diskar** längs hello vänster kant hello virtuella instrumentpanelen under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7042f-109">Click **Disks** along hello left edge of hello virtual machine dashboard, under **Settings**.</span></span>

 <span data-ttu-id="7042f-110">hello virtuella instrumentpanelen visar hello namn och typ för alla anslutna diskar.</span><span class="sxs-lookup"><span data-stu-id="7042f-110">hello virtual machine dashboard lists hello name and type of all attached disks.</span></span> <span data-ttu-id="7042f-111">Den här skärmen visar till exempel en virtuell dator med en operativsystemdisk och en datadisk:</span><span class="sxs-lookup"><span data-stu-id="7042f-111">For example, this screen shows a virtual machine with one operating system (OS) disk and one data disk:</span></span>

    ![Hitta datadisk](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-hello-disk"></a><span data-ttu-id="7042f-113">Koppla bort hello disk</span><span class="sxs-lookup"><span data-stu-id="7042f-113">Detach hello disk</span></span>
1. <span data-ttu-id="7042f-114">Hello Azure-portalen, klicka på **virtuella datorer**, och klicka sedan på hello namnet på hello virtuell dator som har hello datadisk du vill toodetach.</span><span class="sxs-lookup"><span data-stu-id="7042f-114">From hello Azure portal, click **Virtual Machines**, and then click hello name of hello virtual machine that has hello data disk you want toodetach.</span></span>

2. <span data-ttu-id="7042f-115">Klicka på **diskar** längs hello vänster kant hello virtuella instrumentpanelen under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7042f-115">Click **Disks** along hello left edge of hello virtual machine dashboard, under **Settings**.</span></span>

3. <span data-ttu-id="7042f-116">Klicka på hello disk du vill toodetach.</span><span class="sxs-lookup"><span data-stu-id="7042f-116">Click hello disk you want toodetach.</span></span>

  ![Identifiera hello disk toodetach](./media/howto-detach-disk-windows-linux/disklist.png)

4. <span data-ttu-id="7042f-118">Hello kommandofältet klickar du på **Detach**.</span><span class="sxs-lookup"><span data-stu-id="7042f-118">From hello command bar, click **Detach**.</span></span>

  ![Leta upp hello frånkoppling kommando](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. <span data-ttu-id="7042f-120">I fönstret för bekräftelse av hello klickar du på **Ja** toodetach hello disk.</span><span class="sxs-lookup"><span data-stu-id="7042f-120">In hello confirmation window, click **Yes** toodetach hello disk.</span></span>

  ![Bekräfta kopplar bort hello-disk](./media/howto-detach-disk-windows-linux/confirmdetach.png)

<span data-ttu-id="7042f-122">hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7042f-122">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>
