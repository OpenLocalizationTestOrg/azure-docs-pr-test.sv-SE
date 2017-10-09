#### <a name="toostop-and-start-a-virtual-device"></a><span data-ttu-id="f3cd6-101">toostop och starta en virtuell enhet</span><span class="sxs-lookup"><span data-stu-id="f3cd6-101">toostop and start a virtual device</span></span>
<span data-ttu-id="f3cd6-102">toostop en virtuell enhet klickar du på namnet och klicka sedan på **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-102">toostop a virtual device, click its name, and then click **Shutdown**.</span></span> <span data-ttu-id="f3cd6-103">Medan hello virtuella enheten stängs av, är dess status **stoppar**.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-103">While hello virtual device is shutting down, its status is **Stopping**.</span></span> <span data-ttu-id="f3cd6-104">När hello virtuella enheten har avbrutits dess status är **stoppad**.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-104">After hello virtual device is stopped, its status is **Stopped**.</span></span>

<span data-ttu-id="f3cd6-105">Använd följande cmdlet: ar toostop hello och starta en virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-105">Use hello following cmdlets toostop and start a virtual device.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-virtual-device"></a><span data-ttu-id="f3cd6-106">toorestart en virtuell enhet</span><span class="sxs-lookup"><span data-stu-id="f3cd6-106">toorestart a virtual device</span></span>
<span data-ttu-id="f3cd6-107">När en virtuell enhet är igång och du vill toorestart, klickar du på namnet och klickar sedan på **starta om**.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-107">When a virtual device is running and you want toorestart it, click its name, and then click **Restart**.</span></span> <span data-ttu-id="f3cd6-108">Hello virtuella enheten startas om är dess status är **omstart**.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-108">While hello virtual device is restarting, its status is **Restarting**.</span></span> <span data-ttu-id="f3cd6-109">När hello virtuell enhet är redo för du toouse, är dess status **kör**.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-109">When hello virtual device is ready for you toouse, its status is **Running**.</span></span>

<span data-ttu-id="f3cd6-110">Använd hello följande cmdlet toorestart en virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="f3cd6-110">Use hello following cmdlet toorestart a virtual device.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

