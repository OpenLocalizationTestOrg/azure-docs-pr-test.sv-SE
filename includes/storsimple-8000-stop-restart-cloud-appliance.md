#### <a name="to-stop-and-start-a-cloud-appliance"></a><span data-ttu-id="2d775-101">Så här stoppar och startar du en molninstallation</span><span class="sxs-lookup"><span data-stu-id="2d775-101">To stop and start a cloud appliance</span></span>

1. <span data-ttu-id="2d775-102">Om du vill stoppa en molninstallation går du till den virtuella datorn för molninstallationen.</span><span class="sxs-lookup"><span data-stu-id="2d775-102">To stop a cloud appliance, go to the VM for your cloud appliance.</span></span>
    <span data-ttu-id="2d775-103">![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span><span class="sxs-lookup"><span data-stu-id="2d775-103">![StorSimple Cloud Appliance Virtual Machine](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span></span>

2. <span data-ttu-id="2d775-104">Klicka på **Stoppa** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="2d775-104">From the command bar, click **Stop**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. <span data-ttu-id="2d775-106">Klicka på **Ja** när du uppmanas att bekräfta åtgärden.</span><span class="sxs-lookup"><span data-stu-id="2d775-106">When prompted for confirmation, click **Yes**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. <span data-ttu-id="2d775-108">När du stoppar en virtuell dator avallokeras den.</span><span class="sxs-lookup"><span data-stu-id="2d775-108">When you stop a VM, it gets deallocated.</span></span> <span data-ttu-id="2d775-109">När molninstallationen håller på att stoppas visas statusen **Frigör**.</span><span class="sxs-lookup"><span data-stu-id="2d775-109">While the cloud appliance is stopping, its status is **Deallocating**.</span></span> <span data-ttu-id="2d775-110">När molninstallationen har stoppats visas statusen **Stoppad (frigjord)**.</span><span class="sxs-lookup"><span data-stu-id="2d775-110">After the cloud appliance is stopped, its status is **Stopped (deallocated)**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. <span data-ttu-id="2d775-112">När en virtuell dator har stoppats startar du den genom att klicka på **Starta** (knappen blir tillgänglig).</span><span class="sxs-lookup"><span data-stu-id="2d775-112">Once a VM is stopped, click **Start** (button becomes available) to start the VM.</span></span> <span data-ttu-id="2d775-113">När molninstallationen har startats visas statusen **Startad**.</span><span class="sxs-lookup"><span data-stu-id="2d775-113">After the cloud appliance has started up, its status is **Started**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

<span data-ttu-id="2d775-115">Du kan stoppa och starta en molninstallation med följande cmdlets.</span><span class="sxs-lookup"><span data-stu-id="2d775-115">Use the following cmdlets to stop and start a cloud appliance.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-cloud-appliance"></a><span data-ttu-id="2d775-116">Så här startar du om en molninstallation</span><span class="sxs-lookup"><span data-stu-id="2d775-116">To restart a cloud appliance</span></span>

<span data-ttu-id="2d775-117">Om du vill starta om en molninstallation går du till den virtuella datorn för molninstallationen.</span><span class="sxs-lookup"><span data-stu-id="2d775-117">To restart a cloud appliance, go to the VM for your cloud appliance.</span></span> <span data-ttu-id="2d775-118">Klicka på **Starta om** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="2d775-118">From the command bar, click **Restart**.</span></span> <span data-ttu-id="2d775-119">Bekräfta omstarten när du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="2d775-119">When prompted, confirm the restart.</span></span> <span data-ttu-id="2d775-120">När molninstallationen är redo att användas visas statusen **Körs**.</span><span class="sxs-lookup"><span data-stu-id="2d775-120">When the cloud appliance is ready for you to use, its status is **Running**.</span></span>

![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

<span data-ttu-id="2d775-122">Du kan starta om en molninstallation med följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="2d775-122">Use the following cmdlet to restart a cloud appliance.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

