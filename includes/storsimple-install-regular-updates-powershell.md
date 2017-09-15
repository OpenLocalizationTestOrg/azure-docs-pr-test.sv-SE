<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="c70cd-101">Så här installerar du regelbundna uppdateringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="c70cd-101">To install regular updates via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="c70cd-102">Öppna enhetens seriekonsol och välj alternativ 1, **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="c70cd-102">Open the device serial console and select option 1, **Log in with full access**.</span></span> <span data-ttu-id="c70cd-103">Ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c70cd-103">Type the password.</span></span> <span data-ttu-id="c70cd-104">Standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="c70cd-104">The default password is *Password1*.</span></span> 
2. <span data-ttu-id="c70cd-105">Skriv följande vid kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="c70cd-105">At the command prompt, type:</span></span>
   
     `Get-HcsUpdateAvailability`
   
    <span data-ttu-id="c70cd-106">Du meddelas om det finns uppdateringar och om uppdateringarna är störande eller utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="c70cd-106">You will be notified if updates are available and whether the updates are disruptive or non-disruptive.</span></span>
3. <span data-ttu-id="c70cd-107">Skriv följande vid kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="c70cd-107">At the command prompt, type:</span></span>
   
     `Start-HcsUpdate`
   
    <span data-ttu-id="c70cd-108">Uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="c70cd-108">The update process will start.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="c70cd-109">Det här kommandot gäller enbart för regelbundna uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="c70cd-109">This command applies only to regular updates.</span></span> <span data-ttu-id="c70cd-110">Du kör det här kommandot på en enda domänkontrollant, men både domänkontrollanter kommer att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c70cd-110">You run this command on only one controller, but both controllers will be updated.</span></span> 
> * <span data-ttu-id="c70cd-111">Märker du kanske en domänkontrollant och växling vid fel under uppdateringen; växling vid fel påverkar inte filsystemets tillgänglighet eller åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c70cd-111">You may notice a controller failover during the update process; however, the failover will not affect system availability or operation.</span></span>
> 
> 

