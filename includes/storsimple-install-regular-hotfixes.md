<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-regular-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="24084-101">tooinstall reguljära snabbkorrigeringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="24084-101">tooinstall regular hotfixes via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="24084-102">Ansluta toohello enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="24084-102">Connect toohello device serial console.</span></span> <span data-ttu-id="24084-103">Mer information finns i [steg 1: ansluta toohello seriekonsolen](../articles/storsimple/storsimple-update-device.md#step1).</span><span class="sxs-lookup"><span data-stu-id="24084-103">For more information, see [Step 1: Connect toohello serial console](../articles/storsimple/storsimple-update-device.md#step1).</span></span>
2. <span data-ttu-id="24084-104">Välj alternativ 1, i menyn för seriekonsolen av hello **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="24084-104">In hello serial console menu, select option 1, **Log in with full access**.</span></span> <span data-ttu-id="24084-105">Ange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="24084-105">Type hello password.</span></span> <span data-ttu-id="24084-106">hello standardlösenordet är **Password1**.</span><span class="sxs-lookup"><span data-stu-id="24084-106">hello default password is **Password1**.</span></span>
3. <span data-ttu-id="24084-107">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="24084-107">At hello command prompt, type:</span></span>
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > <span data-ttu-id="24084-108">Detta kommando gäller endast tooregular snabbkorrigeringar.</span><span class="sxs-lookup"><span data-stu-id="24084-108">This command applies only tooregular hotfixes.</span></span> <span data-ttu-id="24084-109">Du kör det här kommandot på en enda domänkontrollant, men både domänkontrollanter kommer att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="24084-109">You run this command on only one controller, but both controllers will be updated.</span></span>
    > <span data-ttu-id="24084-110">Märker du kanske en domänkontrollant och växling vid fel under uppdateringsprocessen hello; hello redundans påverkar inte filsystemets tillgänglighet eller åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24084-110">You may notice a controller failover during hello update process; however, hello failover will not affect system availability or operation.</span></span>

4. <span data-ttu-id="24084-111">När du uppmanas ange hello sökvägen toohello delad nätverksmapp som innehåller hello snabbkorrigeringsfiler.</span><span class="sxs-lookup"><span data-stu-id="24084-111">When prompted, supply hello path toohello network shared folder that contains hello hotfix files.</span></span>
5. <span data-ttu-id="24084-112">Du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="24084-112">You will be prompted for confirmation.</span></span> <span data-ttu-id="24084-113">Typen **Y** tooproceed med hello snabbkorrigeringen installation.</span><span class="sxs-lookup"><span data-stu-id="24084-113">Type **Y** tooproceed with hello hotfix installation.</span></span>

