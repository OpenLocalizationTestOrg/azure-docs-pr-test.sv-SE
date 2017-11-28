<!--author=alkohli last changed: 08/21/17-->

#### <a name="to-download-hotfixes"></a><span data-ttu-id="199e1-101">Ladda ned snabbkorrigerar</span><span class="sxs-lookup"><span data-stu-id="199e1-101">To download hotfixes</span></span>

<span data-ttu-id="199e1-102">Utför följande steg för att hämta programuppdateringen från Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="199e1-102">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="199e1-103">Starta Internet Explorer och gå till [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="199e1-103">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="199e1-104">Om det här är första gången du använder Microsoft Update Catalog på den här datorn klickar du på **Installera** när du uppmanas att installera tillägget för Microsoft Update Catalog.</span><span class="sxs-lookup"><span data-stu-id="199e1-104">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>

    ![Installera katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. <span data-ttu-id="199e1-106">I sökrutan i Microsoft Update-katalogen ange Knowledge Base (KB) antalet den snabbkorrigering som du vill hämta, till exempel **4037264**, och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="199e1-106">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download, for example **4037264**, and then click **Search**.</span></span>
   
    <span data-ttu-id="199e1-107">Snabbkorrigeringen listan visas till exempel **kumulativa programvara paket Update 5.0 för StorSimple 8000-serien**.</span><span class="sxs-lookup"><span data-stu-id="199e1-107">The hotfix listing appears, for example, **Cumulative Software Bundle Update 5.0 for StorSimple 8000 Series**.</span></span>
   
    ![Sökkatalog](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. <span data-ttu-id="199e1-109">Klicka på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="199e1-109">Click **Download**.</span></span> <span data-ttu-id="199e1-110">Ange eller **Bläddra** till en lokal plats där du vill att nedladdningarna ska läggas.</span><span class="sxs-lookup"><span data-stu-id="199e1-110">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="199e1-111">Klicka på filer som hämtas till den angivna platsen och mapp.</span><span class="sxs-lookup"><span data-stu-id="199e1-111">Click the files to download to the specified location and folder.</span></span> <span data-ttu-id="199e1-112">Mappen kan också kopieras till en nätverksresurs som kan nås från enheten.</span><span class="sxs-lookup"><span data-stu-id="199e1-112">The folder can also be copied to a network share that is reachable from the device.</span></span>
5. <span data-ttu-id="199e1-113">Sök efter eventuella ytterligare snabbkorrigeringar som anges i tabellen ovan (**4037266**), och hämta motsvarande filer till specifika mappar som anges i tabellen ovan.</span><span class="sxs-lookup"><span data-stu-id="199e1-113">Search for any additional hotfixes listed in the table above (**4037266**), and download the corresponding files to the specific folders as listed in the preceding table.</span></span>

> [!NOTE]
> <span data-ttu-id="199e1-114">Snabbkorrigeringarna måste vara tillgänglig från båda domänkontrollanter för att identifiera eventuella felmeddelanden från peer-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="199e1-114">The hotfixes must be accessible from both controllers to detect any potential error messages from the peer controller.</span></span>
>
> <span data-ttu-id="199e1-115">Snabbkorrigeringarna måste kopieras till tre separata mappar.</span><span class="sxs-lookup"><span data-stu-id="199e1-115">The hotfixes must be copied in 3 separate folders.</span></span> <span data-ttu-id="199e1-116">Exempelvis kan enheten MDS-program/TIS agentuppdatering kopieras i _FirstOrderUpdate_ mapp, alla andra utan avbrott uppdateringar kan kopieras i den _SecondOrderUpdate_ mapp, och Underhåll läge uppdateringar kopierade i _ThirdOrderUpdate_ mapp.</span><span class="sxs-lookup"><span data-stu-id="199e1-116">For example, the device software/Cis/MDS agent update can be copied in _FirstOrderUpdate_ folder, all the other non-disruptive updates could be copied in the _SecondOrderUpdate_ folder, and maintenance mode updates copied in _ThirdOrderUpdate_ folder.</span></span>

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="199e1-117">Installera och verifiera snabbkorrigeringar i normalläge</span><span class="sxs-lookup"><span data-stu-id="199e1-117">To install and verify regular mode hotfixes</span></span>

<span data-ttu-id="199e1-118">Utför följande steg för att installera och verifiera snabbkorrigeringar i normalläge.</span><span class="sxs-lookup"><span data-stu-id="199e1-118">Perform the following steps to install and verify regular-mode hotfixes.</span></span> <span data-ttu-id="199e1-119">Om du redan har installerat dem med hjälp av Azure portal, gå vidare till [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="199e1-119">If you already installed them using the Azure portal, skip ahead to [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="199e1-120">Gå in i Windows PowerShell-gränssnittet på din StorSimple-enhets seriekonsol för att installera snabbkorrigerarna.</span><span class="sxs-lookup"><span data-stu-id="199e1-120">To install the hotfixes, access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="199e1-121">Följ de detaljerade instruktionerna i [Använd PuTTY för att ansluta till enhetens seriekonsol](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="199e1-121">Follow the detailed instructions in [Use PuTTy to connect to the serial console](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="199e1-122">Tryck på **Retur** i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="199e1-122">At the command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="199e1-123">Välj **Alternativ 1** för att logga in på enheten med fullständig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="199e1-123">Select **Option 1** to log on to the device with full access.</span></span> <span data-ttu-id="199e1-124">Vi rekommenderar att du installerar snabbkorrigeringen på den passiva styrenheten först.</span><span class="sxs-lookup"><span data-stu-id="199e1-124">We recommend that you install the hotfix on the passive controller first.</span></span>
3. <span data-ttu-id="199e1-125">Ange följande i kommandotolken för att installera snabbkorrigeringen:</span><span class="sxs-lookup"><span data-stu-id="199e1-125">To install the hotfix, at the command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="199e1-126">Använd IP i stället för DNS i resurssökvägen i kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="199e1-126">Use IP rather than DNS in share path in the above command.</span></span> <span data-ttu-id="199e1-127">Parametern för autentiseringsuppgifter används bara om du ansluter till en autentiserad resurs.</span><span class="sxs-lookup"><span data-stu-id="199e1-127">The credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="199e1-128">Vi rekommenderar att du använder parametern för autentiseringsuppgifter för att få åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="199e1-128">We recommend that you use the credential parameter to access shares.</span></span> <span data-ttu-id="199e1-129">Även resurser som är öppna för ”alla” är vanligtvis inte öppna för icke-autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="199e1-129">Even shares that are open to “everyone” are typically not open to unauthenticated users.</span></span>
   
4. <span data-ttu-id="199e1-130">Ange lösenordet när du uppmanas att göra så.</span><span class="sxs-lookup"><span data-stu-id="199e1-130">Supply the password when prompted.</span></span> <span data-ttu-id="199e1-131">Ett exempel på utdata för att installera första orderns uppdateringar visas nedan.</span><span class="sxs-lookup"><span data-stu-id="199e1-131">A sample output for installing the first order updates is shown below.</span></span> <span data-ttu-id="199e1-132">Du måste peka på filen för den första uppdateringen i ordning.</span><span class="sxs-lookup"><span data-stu-id="199e1-132">For the first order update, you need to point to the specific file.</span></span>

    >[!NOTE] 
    > <span data-ttu-id="199e1-133">Du bör installera den _HcsSoftwareUpdate.exe_ första.</span><span class="sxs-lookup"><span data-stu-id="199e1-133">You should install the _HcsSoftwareUpdate.exe_ first.</span></span> <span data-ttu-id="199e1-134">När installationen har slutförts kan du sedan installera _CisMdsAgentUpdate.exe_.</span><span class="sxs-lookup"><span data-stu-id="199e1-134">After this install has completed, then install _CisMdsAgentUpdate.exe_.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
5. <span data-ttu-id="199e1-135">Skriv **Y** när du uppmanas att bekräfta installationen av snabbkorrigeringen.</span><span class="sxs-lookup"><span data-stu-id="199e1-135">Type **Y** when prompted to confirm the hotfix installation.</span></span>
6. <span data-ttu-id="199e1-136">Övervaka uppdateringen med hjälp av `Get-HcsUpdateStatus`-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="199e1-136">Monitor the update by using the `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="199e1-137">Uppdateringen slutförs först på den passiva styrenheten.</span><span class="sxs-lookup"><span data-stu-id="199e1-137">The update will first complete on the passive controller.</span></span> <span data-ttu-id="199e1-138">När den passiva styrenheten har uppdaterats sker en redundans och uppdateringen tillämpas sedan på den andra styrenheten.</span><span class="sxs-lookup"><span data-stu-id="199e1-138">Once the passive controller is updated, there will be a failover and the update will then get applied on the other controller.</span></span> <span data-ttu-id="199e1-139">Uppdateringen har slutförts när båda styrenheterna har uppdateras.</span><span class="sxs-lookup"><span data-stu-id="199e1-139">The update is complete when both the controllers are updated.</span></span>
   
    <span data-ttu-id="199e1-140">Följande exempel på utdata visar att uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="199e1-140">The following sample output shows the update in progress.</span></span> <span data-ttu-id="199e1-141">Den `RunInprogress` är `True` när uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="199e1-141">The `RunInprogress` is `True` when the update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="199e1-142">Följande exempel på utdata visar att uppdateringen är färdig.</span><span class="sxs-lookup"><span data-stu-id="199e1-142">The following sample output indicates that the update is finished.</span></span> <span data-ttu-id="199e1-143">Den `RunInProgress` är `False` när uppdateringen är klar.</span><span class="sxs-lookup"><span data-stu-id="199e1-143">The `RunInProgress` is `False` when the update is complete.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > <span data-ttu-id="199e1-144">I vissa fall rapporterar cmdlet `False` när uppdateringen fortfarande pågår.</span><span class="sxs-lookup"><span data-stu-id="199e1-144">Occasionally, the cmdlet reports `False` when the update is still in progress.</span></span> <span data-ttu-id="199e1-145">Om du vill kontrollera att snabbkorrigeringen har slutförts väntar du några minuter och sedan kör du det här kommandot igen och kontrollerar att `RunInProgress` är `False`.</span><span class="sxs-lookup"><span data-stu-id="199e1-145">To ensure that the hotfix is complete, wait for a few minutes, rerun this command and verify that the `RunInProgress` is `False`.</span></span> <span data-ttu-id="199e1-146">Om det har det har snabbkorrigeringen slutförts.</span><span class="sxs-lookup"><span data-stu-id="199e1-146">If it is, then the hotfix has completed.</span></span>

7. <span data-ttu-id="199e1-147">När programuppdateringen har slutförts kan du kontrollera systemprogramversionerna.</span><span class="sxs-lookup"><span data-stu-id="199e1-147">After the software update is complete, verify the system software versions.</span></span> <span data-ttu-id="199e1-148">Ange:</span><span class="sxs-lookup"><span data-stu-id="199e1-148">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="199e1-149">Du bör se följande versioner:</span><span class="sxs-lookup"><span data-stu-id="199e1-149">You should see the following versions:</span></span>
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   *  `HcsSoftwareVersion: 6.3.9600.17845`
   
    <span data-ttu-id="199e1-150">Om versionsnumret inte ändras efter att uppdateringen har tillämpats indikerar det att snabbkorrigeringen har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="199e1-150">If the version number does not change after applying the update, it indicates that the hotfix has failed to apply.</span></span> <span data-ttu-id="199e1-151">Kontakta [Microsoft Support](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.</span><span class="sxs-lookup"><span data-stu-id="199e1-151">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) for further assistance.</span></span>
     
    > [!IMPORTANT]
    > <span data-ttu-id="199e1-152">Du måste starta om den aktiva styrenheten via den `Restart-HcsController` cmdlet innan du tillämpar nästa uppdatering.</span><span class="sxs-lookup"><span data-stu-id="199e1-152">You must restart the active controller via the `Restart-HcsController` cmdlet before applying the next update.</span></span>
     
8. <span data-ttu-id="199e1-153">Upprepa steg 3-6 för att installera den _CisMDSAgentupdate.exe_ agent hämtas till din _FirstOrderUpdate_ mapp.</span><span class="sxs-lookup"><span data-stu-id="199e1-153">Repeat steps 3-6 to install the _CisMDSAgentupdate.exe_ agent downloaded to your _FirstOrderUpdate_ folder.</span></span>
8. <span data-ttu-id="199e1-154">Upprepa steg 3 – 6 för att installera uppdateringar för andra ordning.</span><span class="sxs-lookup"><span data-stu-id="199e1-154">Repeat steps 3-6 to install the second order updates.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="199e1-155">För andra uppdateringar, flera uppdateringar installeras genom att bara köra den `Start-HcsHotfix cmdlet` och peka på den mapp där det finns andra uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="199e1-155">For second order updates, multiple updates can be installed by just running the `Start-HcsHotfix cmdlet` and pointing to the folder where second order updates are located.</span></span> <span data-ttu-id="199e1-156">Cmdleten kör alla tillgängliga uppdateringar i mappen.</span><span class="sxs-lookup"><span data-stu-id="199e1-156">The cmdlet will execute all the updates available in the folder.</span></span> <span data-ttu-id="199e1-157">Om en uppdatering redan är installerad identifierar uppdateringslogiken det och tillämpar inte uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="199e1-157">If an update is already installed, the update logic will detect that and not apply that update.</span></span>

    <span data-ttu-id="199e1-158">När alla snabbkorrigeringar har installerats använder du `Get-HcsSystem`-cmdleten.</span><span class="sxs-lookup"><span data-stu-id="199e1-158">After all the hotfixes are installed, use the `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="199e1-159">Versionerna bör vara:</span><span class="sxs-lookup"><span data-stu-id="199e1-159">The versions should be:</span></span>
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="199e1-160">Installera och verifiera snabbkorrigeringar i underhållsläge</span><span class="sxs-lookup"><span data-stu-id="199e1-160">To install and verify maintenance mode hotfixes</span></span>

<span data-ttu-id="199e1-161">Använd KB4037263 att installera uppdateringar av inbyggd disk.</span><span class="sxs-lookup"><span data-stu-id="199e1-161">Use KB4037263 to install disk firmware updates.</span></span> <span data-ttu-id="199e1-162">Det här är störande uppdateringar och tar cirka 30 minuter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="199e1-162">These are disruptive updates and take around 30 minutes to complete.</span></span> <span data-ttu-id="199e1-163">Du kan välja att installera dem i ett planerat underhållsfönster genom att ansluta till enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="199e1-163">You can choose to install these in a planned maintenance window by connecting to the device serial console.</span></span>

> [!NOTE] 
> <span data-ttu-id="199e1-164">Om den inbyggda programvaran disken är redan uppdaterade, behöver du inte installera uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="199e1-164">If your disk firmware is already up-to-date, you won't need to install these updates.</span></span> <span data-ttu-id="199e1-165">Kör `Get-HcsUpdateAvailability`-cmdleten från enhetens seriekonsol för att kontrollera om det finns tillgängliga uppdateringar och om uppdateringarna är störande (underhållsläge) eller avbrottsfria (standardläget).</span><span class="sxs-lookup"><span data-stu-id="199e1-165">Run the `Get-HcsUpdateAvailability` cmdlet from the device serial console to check if updates are available and whether the updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="199e1-166">Följ anvisningarna nedan om du vill installera uppdateringarna för den inbyggda programvaran för disken.</span><span class="sxs-lookup"><span data-stu-id="199e1-166">To install the disk firmware updates, follow the instructions below.</span></span>

1. <span data-ttu-id="199e1-167">Sätt enheten i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="199e1-167">Place the device in the maintenance mode.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="199e1-168">Använd inte Windows PowerShell-fjärrkommunikation när du ansluter till en enhet i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="199e1-168">Do not use Windows PowerShell remoting when connecting to a device in maintenance mode.</span></span> <span data-ttu-id="199e1-169">I stället köra denna cmdlet på styrenheten för enheten när du är ansluten till enhetens seriekonsol.</span><span class="sxs-lookup"><span data-stu-id="199e1-169">Instead run this cmdlet on the device controller when connected through the device serial console.</span></span>

    <span data-ttu-id="199e1-170">Om du vill placera styrenheten i underhållsläge, skriver du:</span><span class="sxs-lookup"><span data-stu-id="199e1-170">To place the controller in maintenance mode, type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="199e1-171">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="199e1-171">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="199e1-172">Båda styrenheterna och starta sedan om i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="199e1-172">Both the controllers then restart into maintenance mode.</span></span>
2. <span data-ttu-id="199e1-173">Ange följande för att installera uppdateringen av inbyggd programvara för disk:</span><span class="sxs-lookup"><span data-stu-id="199e1-173">To install the disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="199e1-174">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="199e1-174">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. <span data-ttu-id="199e1-175">Övervaka installationsförloppet med `Get-HcsUpdateStatus`-kommandot.</span><span class="sxs-lookup"><span data-stu-id="199e1-175">Monitor the install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="199e1-176">Uppdateringen är slutförd när `RunInProgress` ändras till `False`.</span><span class="sxs-lookup"><span data-stu-id="199e1-176">The update is complete when the `RunInProgress` changes to `False`.</span></span>
4. <span data-ttu-id="199e1-177">När installationen är färdig startas styrenheten som snabbkorrigeringen i underhållsläge installerades på om.</span><span class="sxs-lookup"><span data-stu-id="199e1-177">After the installation is complete, the controller on which the maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="199e1-178">Logga in som alternativ 1 med fullständig åtkomst och verifiera versionen för den inbyggda programvaran för disken.</span><span class="sxs-lookup"><span data-stu-id="199e1-178">Log in as option 1 with full access and verify the disk firmware version.</span></span> <span data-ttu-id="199e1-179">Ange:</span><span class="sxs-lookup"><span data-stu-id="199e1-179">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="199e1-180">De förväntade versionerna för den inbyggda programvaran för disken är:</span><span class="sxs-lookup"><span data-stu-id="199e1-180">The expected disk firmware versions are:</span></span>
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   <span data-ttu-id="199e1-181">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="199e1-181">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    <span data-ttu-id="199e1-182">Kör `Get-HcsFirmwareVersion`-kommandot på den andra styrenheten för att verifiera att programvaruversionen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="199e1-182">Run the `Get-HcsFirmwareVersion` command on the second controller to verify that the software version has been updated.</span></span> <span data-ttu-id="199e1-183">Du kan sedan avsluta underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="199e1-183">You can then exit the maintenance mode.</span></span> <span data-ttu-id="199e1-184">För att göra det anger du följande kommando för varje enhetsstyrenhet:</span><span class="sxs-lookup"><span data-stu-id="199e1-184">To do so, type the following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`

5. <span data-ttu-id="199e1-185">Styrenheterna startas om när du avslutar underhållsläget.</span><span class="sxs-lookup"><span data-stu-id="199e1-185">The controllers restart when you exit maintenance mode.</span></span> <span data-ttu-id="199e1-186">Efter den inbyggda programvaran disk uppdateringar har tillämpats och enheten avslutat underhållsläget, gå tillbaka till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="199e1-186">After the disk firmware updates are successfully applied and the device has exited maintenance mode, return to the Azure portal.</span></span> <span data-ttu-id="199e1-187">Observera att portalen kanske inte visar att du har installerat uppdateringarna i underhållsläge på 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="199e1-187">Note that the portal might not show that you installed the maintenance mode updates for 24 hours.</span></span>

