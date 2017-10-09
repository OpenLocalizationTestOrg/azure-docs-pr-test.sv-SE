<!--author=SharS last changed: 03/17/2016-->

#### <a name="toodownload-hotfixes"></a>toodownload snabbkorrigeringar
Utför följande steg toodownload hello programuppdateringen hello.

1. Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.
    ![Installera katalog](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)
3. Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload, till exempel **3063418**, och klicka sedan på **Sök**.
4. Du ser hello **StorSimple uppdatering 1.2 enhet uppdatering** paket. Klicka på **Lägg till**. hello uppdateringen läggs toohello korg.
5. Sök efter eventuella ytterligare snabbkorrigeringar som anges i hello tabellen ovan (**3043005** och **3063416**), och Lägg till varje hello korg.
6. Klicka på **View Basket** (Visa korg).
   
    ![Visa korg](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. Klicka på **Hämta**. Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear. hello uppdateringarna laddas toohello angav plats och placeras i en undermapp med samma namn som hello uppdatering hello. hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.

> [!NOTE]
> hello snabbkorrigeringar måste vara tillgänglig från båda domänkontrollanterna toodetect eventuella eventuella felmeddelanden från hello peer-domänkontrollant.
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall och kontrollera standardläget snabbkorrigeringar
Utför följande steg tooinstall hello och kontrollera hello regular läge snabbkorrigeringar. Om du redan installerat med hello Azure-portalen kan du gå vidare för[installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello programuppdateringar, åtkomst hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol. Följ hello detaljerade instruktioner i [använda PuTTy tooconnect toohello seriekonsolen](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Hello Kommandotolken, tryck på **RETUR**.
2. Välj **alternativ 1** toolog på toohello enhet med fullständig åtkomst.
3. tooinstall hello uppdateringspaketet, Kommandotolken hello typ:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Använd IP i stället för DNS på resurssökvägen i hello ovanstående kommando. hello autentiseringsuppgiftsparametern används bara om du ansluter till en autentiserad resurs.
   
    Vi rekommenderar att du använder hello autentiseringsuppgifter parametern tooaccess resurser. Även resurser som är öppna för ”alla” är vanligtvis inte att öppna toounauthenticated användare.
   
    Ett exempel på utdata visas nedan.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts hello hotfix installation and could reboot one or
    both of hello controllers. If hello device is serving I/Os, these will not
    be disrupted. Are you sure you want toocontinue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. Typen **Y** när begärd tooconfirm hello snabbkorrigeringsinstallationen.
5. Övervaka hello uppdateringen med hjälp av hello `Get-HcsUpdateStatus` cmdlet.
   
    hello visar följande exempel på utdata hello uppdatering pågår. Hej `RunInprogress` blir `True` när hello uppdatering pågår.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     följande exempel på utdata hello anger hello uppdateringen är klar. Hej `RunInProgress` blir `False` när hello uppdateringen har slutförts.

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > Ibland kan hello cmdlet rapporter `False` när hello uppdateringen pågår fortfarande. tooensure som hello snabbkorrigeringen är klar, Vänta några minuter, kör kommandot och kontrollera att hello `RunInProgress` är `False`. Om det är har hello snabbkorrigeringen slutförts.
   > 
   > 
6. Kontrollera hello system programvaruversioner när hello programuppdateringen har slutförts. Skriv följande kommando hello:
   
    `Get-HcsSystem`
   
    Du bör se hello följande versioner:
   
   * HcsSoftwareVersion: 6.3.9600.17584
   * CisAgentVersion: 1.0.9049.0
   * MdsAgentVersion: 26.0.4696.1433
     
     Om hello versionsnummer inte ändras när hello uppdatering, visar hello snabbkorrigeringen har misslyckats tooapply. Kontakta [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.
7. Upprepa steg 3-5 tooinstall hello återstående regular läge snabbkorrigering (KB3043005).

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall och kontrollera Underhåll läge snabbkorrigeringar
Använda KB3063416 tooinstall disk firmware-uppdateringar. Dessa är störande uppdateringar och ta runt 30 45 minuter toocomplete. Du kan välja tooinstall dessa i ett fönster för planerat underhåll av anslutande toohello enhetens seriekonsol.

uppdateringar av tooinstall hello disk inbyggd, följ instruktionerna för hello nedan.

1. Placera hello enhet i underhållsläge. Observera att du inte bör använda Windows PowerShell-fjärrkommunikation om du ansluter tooa enhet i underhållsläge. Du behöver toorun denna cmdlet på hello enhetskontroll när ansluten via hello enhetens seriekonsol. Ange:
   
    `Enter-HcsMaintenanceMode`
   
    Ett exempel på utdata visas nedan.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Båda hello domänkontrollanter starta sedan om i underhållsläge.
2. tooinstall hello disk firmware-uppdatering, typ:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Ett exempel på utdata visas nedan.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. Övervakaren hello installera förlopp med hjälp av `Get-HcsUpdateStatus` kommando. hello uppdateringen är klar när hello `RunInProgress` ändras för`False`.
4. När hello installationen är klar, hello domänkontrollant som hello Underhåll läge snabbkorrigeringen har installerats måste startas om. Logga in som alternativ 1 med fullständig åtkomst och kontrollera hello disk version på inbyggd programvara. Ange:
   
   `Get-HcsFirmwareVersion`
   
   hello förväntas disk firmware versioner är:
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   Kör hello `Get-HcsFirmwareVersion` kommandot på hello andra domänkontrollanter tooverify som hello programvaruversionen har uppdaterats. Du kan avsluta hello underhållsläge. Skriv följande kommando för varje enhet controller hello:
   
   `Exit-HcsMaintenanceMode`
5. hello domänkontrollanter startas om när du avsluta underhållsläget. Efter hello disk firmware uppdateringar har tillämpats och hello enheten avslutat underhållsläget, returnerar toohello klassiska Azure-portalen. Observera att hello portal inte kanske visar att du installerat hello Underhåll läge uppdateringarna i 24 timmar.

