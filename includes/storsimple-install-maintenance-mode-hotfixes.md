<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="e9b9e-101">Installera Underhåll läge snabbkorrigeringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="e9b9e-101">To install Maintenance mode hotfixes via Windows PowerShell for StorSimple</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e9b9e-102">Du måste först installera snabbkorrigeringen på en domänkontrollant och sedan på den andra styrenheten i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-102">In Maintenance mode, you need to apply the hotfix first on one controller and then on the other controller.</span></span>
> 
> 

1. <span data-ttu-id="e9b9e-103">Placera enheten i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-103">Place the device into Maintenance mode.</span></span> <span data-ttu-id="e9b9e-104">Se [steg 2: Ange underhållsläge](../articles/storsimple/storsimple-update-device.md#step2) instruktioner om hur du anger underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-104">See [Step 2: Enter Maintenance mode](../articles/storsimple/storsimple-update-device.md#step2) for instructions on how to enter Maintenance mode.</span></span>
2. <span data-ttu-id="e9b9e-105">Om du vill installera snabbkorrigeringen, skriver du:</span><span class="sxs-lookup"><span data-stu-id="e9b9e-105">To apply the hotfix, type:</span></span>
   
     `Start-HcsHotfix` 
3. <span data-ttu-id="e9b9e-106">När du uppmanas, ange sökvägen till den delade nätverksmappen som innehåller snabbkorrigeringsfilerna.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-106">When prompted, supply the path to the network shared folder that contains the hotfix files.</span></span>
4. <span data-ttu-id="e9b9e-107">Du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-107">You will be prompted for confirmation.</span></span> <span data-ttu-id="e9b9e-108">Typen **Y** att fortsätta med installationen av snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-108">Type **Y** to proceed with the hotfix installation.</span></span>
5. <span data-ttu-id="e9b9e-109">När du har installerat snabbkorrigeringen på en domänkontrollant, kan du logga in på den andra styrenheten.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-109">After you have applied the hotfix on one controller, log on to the other controller.</span></span> <span data-ttu-id="e9b9e-110">Installera snabbkorrigeringen som du skapade tidigare domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-110">Apply the hotfix as you did for the previous controller.</span></span>
6. <span data-ttu-id="e9b9e-111">Avsluta underhållsläget när snabbkorrigeringarna tillämpas.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-111">After the hotfixes are applied, exit Maintenance mode.</span></span> <span data-ttu-id="e9b9e-112">Se [steg 4: avsluta underhållsläget](../articles/storsimple/storsimple-update-device.md#step4) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="e9b9e-112">See [Step 4: Exit Maintenance mode](../articles/storsimple/storsimple-update-device.md#step4) for instructions.</span></span>

