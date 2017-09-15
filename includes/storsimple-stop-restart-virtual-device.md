#### <a name="to-stop-and-start-a-virtual-device"></a><span data-ttu-id="f91b7-101">Stoppa och starta en virtuell enhet</span><span class="sxs-lookup"><span data-stu-id="f91b7-101">To stop and start a virtual device</span></span>
<span data-ttu-id="f91b7-102">Om du vill stoppa en virtuell enhet, klickar du på namnet och klickar sedan på **Stäng av**.</span><span class="sxs-lookup"><span data-stu-id="f91b7-102">To stop a virtual device, click its name, and then click **Shutdown**.</span></span> <span data-ttu-id="f91b7-103">När den virtuella enheten stängs av, är dess status **Stoppar**.</span><span class="sxs-lookup"><span data-stu-id="f91b7-103">While the virtual device is shutting down, its status is **Stopping**.</span></span> <span data-ttu-id="f91b7-104">När den virtuella enheten har stängts av, är dess status **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="f91b7-104">After the virtual device is stopped, its status is **Stopped**.</span></span>

<span data-ttu-id="f91b7-105">Du kan använda följande cmdletar för att stoppa och starta en virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="f91b7-105">Use the following cmdlets to stop and start a virtual device.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-virtual-device"></a><span data-ttu-id="f91b7-106">Starta om en virtuell enhet</span><span class="sxs-lookup"><span data-stu-id="f91b7-106">To restart a virtual device</span></span>
<span data-ttu-id="f91b7-107">När en virtuell enhet är igång och du vill starta om den, klickar du på dess namn och klickar sedan på **Starta om**.</span><span class="sxs-lookup"><span data-stu-id="f91b7-107">When a virtual device is running and you want to restart it, click its name, and then click **Restart**.</span></span> <span data-ttu-id="f91b7-108">Medan den virtuella enheten startas om är dess status **Startar om**.</span><span class="sxs-lookup"><span data-stu-id="f91b7-108">While the virtual device is restarting, its status is **Restarting**.</span></span> <span data-ttu-id="f91b7-109">När den virtuella enheten är klar för användning, är dess status **Igång**.</span><span class="sxs-lookup"><span data-stu-id="f91b7-109">When the virtual device is ready for you to use, its status is **Running**.</span></span>

<span data-ttu-id="f91b7-110">Du kan använda följande cmdletar för att starta om en virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="f91b7-110">Use the following cmdlet to restart a virtual device.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

