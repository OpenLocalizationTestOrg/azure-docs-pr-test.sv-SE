#### <a name="toostop-and-start-a-cloud-appliance"></a><span data-ttu-id="4d77c-101">toostop och starta en moln-installation</span><span class="sxs-lookup"><span data-stu-id="4d77c-101">toostop and start a cloud appliance</span></span>

1. <span data-ttu-id="4d77c-102">toostop en moln-enhet går toohello VM för din enhet i molnet.</span><span class="sxs-lookup"><span data-stu-id="4d77c-102">toostop a cloud appliance, go toohello VM for your cloud appliance.</span></span>
    <span data-ttu-id="4d77c-103">![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span><span class="sxs-lookup"><span data-stu-id="4d77c-103">![StorSimple Cloud Appliance Virtual Machine](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span></span>

2. <span data-ttu-id="4d77c-104">Hello kommandofältet klickar du på **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="4d77c-104">From hello command bar, click **Stop**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. <span data-ttu-id="4d77c-106">Klicka på **Ja** när du uppmanas att bekräfta åtgärden.</span><span class="sxs-lookup"><span data-stu-id="4d77c-106">When prompted for confirmation, click **Yes**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. <span data-ttu-id="4d77c-108">När du stoppar en virtuell dator avallokeras den.</span><span class="sxs-lookup"><span data-stu-id="4d77c-108">When you stop a VM, it gets deallocated.</span></span> <span data-ttu-id="4d77c-109">Hello molnet installation stoppas dess status är **Deallocating**.</span><span class="sxs-lookup"><span data-stu-id="4d77c-109">While hello cloud appliance is stopping, its status is **Deallocating**.</span></span> <span data-ttu-id="4d77c-110">När hello molnet installation har avbrutits dess status är **Stoppad (frigjord)**.</span><span class="sxs-lookup"><span data-stu-id="4d77c-110">After hello cloud appliance is stopped, its status is **Stopped (deallocated)**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. <span data-ttu-id="4d77c-112">När en virtuell dator har stoppats, klickar du på **starta** (knappen blir tillgänglig) toostart hello VM.</span><span class="sxs-lookup"><span data-stu-id="4d77c-112">Once a VM is stopped, click **Start** (button becomes available) toostart hello VM.</span></span> <span data-ttu-id="4d77c-113">När hello molnet enheten har startats dess status är **igång**.</span><span class="sxs-lookup"><span data-stu-id="4d77c-113">After hello cloud appliance has started up, its status is **Started**.</span></span>

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

<span data-ttu-id="4d77c-115">Använd följande cmdlet: ar toostop hello och starta en moln-installation.</span><span class="sxs-lookup"><span data-stu-id="4d77c-115">Use hello following cmdlets toostop and start a cloud appliance.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-cloud-appliance"></a><span data-ttu-id="4d77c-116">toorestart en moln-installation</span><span class="sxs-lookup"><span data-stu-id="4d77c-116">toorestart a cloud appliance</span></span>

<span data-ttu-id="4d77c-117">toorestart en moln-enhet går toohello VM för din enhet i molnet.</span><span class="sxs-lookup"><span data-stu-id="4d77c-117">toorestart a cloud appliance, go toohello VM for your cloud appliance.</span></span> <span data-ttu-id="4d77c-118">Hello kommandofältet klickar du på **starta om**.</span><span class="sxs-lookup"><span data-stu-id="4d77c-118">From hello command bar, click **Restart**.</span></span> <span data-ttu-id="4d77c-119">När du uppmanas bekräfta hello omstart.</span><span class="sxs-lookup"><span data-stu-id="4d77c-119">When prompted, confirm hello restart.</span></span> <span data-ttu-id="4d77c-120">När hello molnet installation är klar för du toouse, är dess status **kör**.</span><span class="sxs-lookup"><span data-stu-id="4d77c-120">When hello cloud appliance is ready for you toouse, its status is **Running**.</span></span>

![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

<span data-ttu-id="4d77c-122">Använd hello följande cmdlet toorestart en moln-installation.</span><span class="sxs-lookup"><span data-stu-id="4d77c-122">Use hello following cmdlet toorestart a cloud appliance.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

