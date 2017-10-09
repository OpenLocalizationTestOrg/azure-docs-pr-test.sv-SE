<!--author=SharS last changed: 11/18/16-->

#### <a name="tooinstall-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="c4afa-101">tooinstall regelbundna uppdateringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="c4afa-101">tooinstall regular updates via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="c4afa-102">Öppna hello enhetens seriekonsol och välj alternativ 1, **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="c4afa-102">Open hello device serial console and select option 1, **Log in with full access**.</span></span> <span data-ttu-id="c4afa-103">Ange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="c4afa-103">Type hello password.</span></span> <span data-ttu-id="c4afa-104">hello standardlösenordet är *Password1*.</span><span class="sxs-lookup"><span data-stu-id="c4afa-104">hello default password is *Password1*.</span></span> 
2. <span data-ttu-id="c4afa-105">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="c4afa-105">At hello command prompt, type:</span></span>
   
     `Get-HcsUpdateAvailability`
   
    <span data-ttu-id="c4afa-106">Du meddelas om det finns uppdateringar och om hello uppdateringar är störande eller utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="c4afa-106">You will be notified if updates are available and whether hello updates are disruptive or non-disruptive.</span></span>
3. <span data-ttu-id="c4afa-107">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="c4afa-107">At hello command prompt, type:</span></span>
   
     `Start-HcsUpdate`
   
    <span data-ttu-id="c4afa-108">hello uppdateringen startar.</span><span class="sxs-lookup"><span data-stu-id="c4afa-108">hello update process will start.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="c4afa-109">Detta kommando gäller endast tooregular uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="c4afa-109">This command applies only tooregular updates.</span></span> <span data-ttu-id="c4afa-110">Du kör det här kommandot på en enda domänkontrollant, men både domänkontrollanter kommer att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c4afa-110">You run this command on only one controller, but both controllers will be updated.</span></span> 
> * <span data-ttu-id="c4afa-111">Märker du kanske en domänkontrollant och växling vid fel under uppdateringsprocessen hello; hello redundans påverkar inte filsystemets tillgänglighet eller åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c4afa-111">You may notice a controller failover during hello update process; however, hello failover will not affect system availability or operation.</span></span>
> 
> 

