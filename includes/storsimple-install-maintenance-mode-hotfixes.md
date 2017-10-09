<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="d381c-101">tooinstall Underhåll läge snabbkorrigeringar via Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="d381c-101">tooinstall Maintenance mode hotfixes via Windows PowerShell for StorSimple</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d381c-102">I underhållsläge, du måste först tooapply hello snabbkorrigeringen på en domänkontrollant och sedan hello andra domänkontrollanter på.</span><span class="sxs-lookup"><span data-stu-id="d381c-102">In Maintenance mode, you need tooapply hello hotfix first on one controller and then on hello other controller.</span></span>
> 
> 

1. <span data-ttu-id="d381c-103">Placera hello enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="d381c-103">Place hello device into Maintenance mode.</span></span> <span data-ttu-id="d381c-104">Se [steg 2: Ange underhållsläge](../articles/storsimple/storsimple-update-device.md#step2) anvisningar för hur tooenter underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="d381c-104">See [Step 2: Enter Maintenance mode](../articles/storsimple/storsimple-update-device.md#step2) for instructions on how tooenter Maintenance mode.</span></span>
2. <span data-ttu-id="d381c-105">tooapply hello snabbkorrigering, typ:</span><span class="sxs-lookup"><span data-stu-id="d381c-105">tooapply hello hotfix, type:</span></span>
   
     `Start-HcsHotfix` 
3. <span data-ttu-id="d381c-106">När du uppmanas ange hello sökvägen toohello delad nätverksmapp som innehåller hello snabbkorrigeringsfiler.</span><span class="sxs-lookup"><span data-stu-id="d381c-106">When prompted, supply hello path toohello network shared folder that contains hello hotfix files.</span></span>
4. <span data-ttu-id="d381c-107">Du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="d381c-107">You will be prompted for confirmation.</span></span> <span data-ttu-id="d381c-108">Typen **Y** tooproceed med hello snabbkorrigeringen installation.</span><span class="sxs-lookup"><span data-stu-id="d381c-108">Type **Y** tooproceed with hello hotfix installation.</span></span>
5. <span data-ttu-id="d381c-109">Efter installation av hello snabbkorrigeringen på en domänkontrollant, logga in toohello andra styrenheten.</span><span class="sxs-lookup"><span data-stu-id="d381c-109">After you have applied hello hotfix on one controller, log on toohello other controller.</span></span> <span data-ttu-id="d381c-110">Snabbkorrigeringen hello som du gjorde för tidigare hello-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="d381c-110">Apply hello hotfix as you did for hello previous controller.</span></span>
6. <span data-ttu-id="d381c-111">Avsluta underhållsläget när hello snabbkorrigeringarna tillämpas.</span><span class="sxs-lookup"><span data-stu-id="d381c-111">After hello hotfixes are applied, exit Maintenance mode.</span></span> <span data-ttu-id="d381c-112">Se [steg 4: avsluta underhållsläget](../articles/storsimple/storsimple-update-device.md#step4) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="d381c-112">See [Step 4: Exit Maintenance mode](../articles/storsimple/storsimple-update-device.md#step4) for instructions.</span></span>

