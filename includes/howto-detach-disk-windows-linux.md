<span data-ttu-id="20334-101">När du inte längre behöver en datadisk som är ansluten till en virtuell dator kan du enkelt koppla bort den.</span><span class="sxs-lookup"><span data-stu-id="20334-101">When you no longer need a data disk that's attached to a virtual machine, you can easily detach it.</span></span> <span data-ttu-id="20334-102">När du kopplar bort en disk tar du bort den från den virtuella datorn, men du tar inte bort den från Azure-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="20334-102">Detaching a disk removes the disk from the virtual machine, but doesn't delete the disk from the Azure storage account.</span></span>

<span data-ttu-id="20334-103">Om du vill använda befintliga data på disken igen kan du ansluta den igen till samma virtuella dator, eller till en annan.</span><span class="sxs-lookup"><span data-stu-id="20334-103">If you want to use the existing data on the disk again, you can reattach it to the same virtual machine, or another one.</span></span>  

> [!NOTE]
> <span data-ttu-id="20334-104">För att koppla bort en operativsystemdisk måste du först ta bort den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="20334-104">To detach an operating system disk, you first need to delete the virtual machine.</span></span>
>

## <a name="find-the-disk"></a><span data-ttu-id="20334-105">Hitta disken</span><span class="sxs-lookup"><span data-stu-id="20334-105">Find the disk</span></span>
<span data-ttu-id="20334-106">Följ de här stegen om du inte känner till namnet på disken eller vill kontrollera det innan du kopplar från den.</span><span class="sxs-lookup"><span data-stu-id="20334-106">If you don't know the name of the disk or want to verify it before you detach it, follow these steps.</span></span>

1. <span data-ttu-id="20334-107">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="20334-107">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="20334-108">Klicka på **Virtual Machines** och välj sedan den lämpliga virtuella enheten.</span><span class="sxs-lookup"><span data-stu-id="20334-108">Click **Virtual Machines**, and then select the appropriate VM.</span></span>

3. <span data-ttu-id="20334-109">Klicka på **Diskar** under **Inställningar** längs vänsterkanten på instrumentpanelen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="20334-109">Click **Disks** along the left edge of the virtual machine dashboard, under **Settings**.</span></span>

 <span data-ttu-id="20334-110">Instrumentpanelen för den virtuella datorn visar namn och typ för alla anslutna diskar.</span><span class="sxs-lookup"><span data-stu-id="20334-110">The virtual machine dashboard lists the name and type of all attached disks.</span></span> <span data-ttu-id="20334-111">Den här skärmen visar till exempel en virtuell dator med en operativsystemdisk och en datadisk:</span><span class="sxs-lookup"><span data-stu-id="20334-111">For example, this screen shows a virtual machine with one operating system (OS) disk and one data disk:</span></span>

    ![Hitta datadisk](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-the-disk"></a><span data-ttu-id="20334-113">Koppla från disken</span><span class="sxs-lookup"><span data-stu-id="20334-113">Detach the disk</span></span>
1. <span data-ttu-id="20334-114">I Azure Portal klickar du på **Virtuella datorer** och sedan på namnet på den virtuella dator som har datadisken som du vill koppla från.</span><span class="sxs-lookup"><span data-stu-id="20334-114">From the Azure portal, click **Virtual Machines**, and then click the name of the virtual machine that has the data disk you want to detach.</span></span>

2. <span data-ttu-id="20334-115">Klicka på **Diskar** under **Inställningar** längs vänsterkanten på instrumentpanelen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="20334-115">Click **Disks** along the left edge of the virtual machine dashboard, under **Settings**.</span></span>

3. <span data-ttu-id="20334-116">Klicka på den disk som du vill koppla från.</span><span class="sxs-lookup"><span data-stu-id="20334-116">Click the disk you want to detach.</span></span>

  ![Identifiera disken du vill koppla från](./media/howto-detach-disk-windows-linux/disklist.png)

4. <span data-ttu-id="20334-118">Klicka på **Koppla från** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="20334-118">From the command bar, click **Detach**.</span></span>

  ![Leta upp kommandot för att koppla från](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. <span data-ttu-id="20334-120">Klicka på **Ja** i bekräftelsefönstret för att koppla från disken.</span><span class="sxs-lookup"><span data-stu-id="20334-120">In the confirmation window, click **Yes** to detach the disk.</span></span>

  ![Bekräfta frånkoppling av disken](./media/howto-detach-disk-windows-linux/confirmdetach.png)

<span data-ttu-id="20334-122">Disken finns kvar i lagringsutrymmet men är inte längre kopplad till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="20334-122">The disk remains in storage but is no longer attached to a virtual machine.</span></span>
