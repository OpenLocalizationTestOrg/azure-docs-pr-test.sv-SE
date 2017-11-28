<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="c86ff-101">Installera reguljära snabbkorrigeringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="c86ff-101">To install regular hotfixes via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="c86ff-102">Ansluta till enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="c86ff-102">Connect to the device serial console.</span></span> <span data-ttu-id="c86ff-103">Mer information finns i [steg 1: ansluta till seriekonsolen](../articles/storsimple/storsimple-update-device.md#step1).</span><span class="sxs-lookup"><span data-stu-id="c86ff-103">For more information, see [Step 1: Connect to the serial console](../articles/storsimple/storsimple-update-device.md#step1).</span></span>
2. <span data-ttu-id="c86ff-104">Välj alternativ 1, i menyn för seriekonsolen **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="c86ff-104">In the serial console menu, select option 1, **Log in with full access**.</span></span> <span data-ttu-id="c86ff-105">Ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c86ff-105">Type the password.</span></span> <span data-ttu-id="c86ff-106">Standardlösenordet är **Password1**.</span><span class="sxs-lookup"><span data-stu-id="c86ff-106">The default password is **Password1**.</span></span>
3. <span data-ttu-id="c86ff-107">Skriv följande vid kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="c86ff-107">At the command prompt, type:</span></span>
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > <span data-ttu-id="c86ff-108">Det här kommandot gäller enbart för vanliga snabbkorrigeringar.</span><span class="sxs-lookup"><span data-stu-id="c86ff-108">This command applies only to regular hotfixes.</span></span> <span data-ttu-id="c86ff-109">Du kör det här kommandot på en enda domänkontrollant, men både domänkontrollanter kommer att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c86ff-109">You run this command on only one controller, but both controllers will be updated.</span></span>
    > <span data-ttu-id="c86ff-110">Märker du kanske en domänkontrollant och växling vid fel under uppdateringen; växling vid fel påverkar inte filsystemets tillgänglighet eller åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c86ff-110">You may notice a controller failover during the update process; however, the failover will not affect system availability or operation.</span></span>

4. <span data-ttu-id="c86ff-111">När du uppmanas, ange sökvägen till den delade nätverksmappen som innehåller snabbkorrigeringsfilerna.</span><span class="sxs-lookup"><span data-stu-id="c86ff-111">When prompted, supply the path to the network shared folder that contains the hotfix files.</span></span>
5. <span data-ttu-id="c86ff-112">Du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="c86ff-112">You will be prompted for confirmation.</span></span> <span data-ttu-id="c86ff-113">Typen **Y** att fortsätta med installationen av snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="c86ff-113">Type **Y** to proceed with the hotfix installation.</span></span>

