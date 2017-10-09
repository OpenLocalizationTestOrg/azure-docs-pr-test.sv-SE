<!--author=alkohli last changed: 09/01/16-->

#### <a name="toodownload-hotfixes"></a>toodownload snabbkorrigeringar
Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.

1. Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.
    ![Installera katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload, till exempel **3186843**, och klicka sedan på **Sök**.
   
    hello snabbkorrigeringen visas, till exempel **kumulativa programvara paket Update 3.0 för StorSimple 8000-serien**.
   
    ![Sökkatalog](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. Klicka på **Lägg till**. hello uppdateringen läggs toohello korg.
5. Sök efter eventuella ytterligare snabbkorrigeringar som anges i hello tabellen ovan (**3186859**), och Lägg till varje toohello korg.
6. Klicka på **View Basket** (Visa korg).
7. Klicka på **Hämta**. Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear. hello uppdateringarna laddas toohello angav plats och placeras i en undermapp med samma namn som hello uppdatering hello. hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.

> [!NOTE]
> hello snabbkorrigeringar måste vara tillgänglig från båda domänkontrollanterna toodetect eventuella eventuella felmeddelanden från hello peer-domänkontrollant.
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall och kontrollera standardläget snabbkorrigeringar
Utför följande steg tooinstall hello och kontrollera snabbkorrigeringar regular-läge. Om du redan installerat med hello Azure-portalen kan du gå vidare för[installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello snabbkorrigeringar, åtkomst hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol. Följ hello detaljerade instruktioner i [använda PuTTy tooconnect toohello seriekonsolen](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Hello Kommandotolken, tryck på **RETUR**.
2. Välj **alternativ 1** toolog på toohello enhet med fullständig åtkomst. Vi rekommenderar att du installerar snabbkorrigeringen hello på hello passiva domänkontrollant först.
3. snabbkorrigering för tooinstall hello, Kommandotolken hello typ:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Använd IP i stället för DNS på resurssökvägen i hello ovanstående kommando. hello autentiseringsuppgiftsparametern används bara om du ansluter till en autentiserad resurs.
   
    Vi rekommenderar att du använder hello autentiseringsuppgifter parametern tooaccess resurser. Även resurser som är öppna för ”alla” är vanligtvis inte att öppna toounauthenticated användare.
   
    Ange hello lösenord när du uppmanas.
   
    Ett exempel på utdata visas nedan.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. Typen **Y** när begärd tooconfirm hello snabbkorrigeringsinstallationen.
5. Övervaka hello uppdateringen med hjälp av hello `Get-HcsUpdateStatus` cmdlet. hello-update först slutför på hello passiva domänkontrollant. När hello passiva controller har uppdaterats, finns en växling vid fel och hello uppdatering används sedan på hello andra styrenheten. hello uppdateringen är klar när båda hello domänkontrollanter uppdateras.
   
    hello visar följande exempel på utdata hello uppdatering pågår. Hej `RunInprogress` blir `True` när hello uppdatering pågår.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     följande exempel på utdata hello anger hello uppdateringen är klar. Hej `RunInProgress` blir `False` när hello uppdateringen har slutförts.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > Ibland kan hello cmdlet rapporter `False` när hello uppdateringen pågår fortfarande. tooensure som hello snabbkorrigeringen är klar, Vänta några minuter, kör kommandot och kontrollera att hello `RunInProgress` är `False`. Om det är har hello snabbkorrigeringen slutförts.

1. Kontrollera hello system programvaruversioner när hello programuppdateringen har slutförts. Ange:
   
    `Get-HcsSystem`
   
    Du bör se hello följande versioner:
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     Om hello versionsnummer inte ändras när hello uppdatering, visar hello snabbkorrigeringen har misslyckats tooapply. Kontakta [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.
     
     > [!IMPORTANT]
     > Du måste starta om hello aktiva styrenheten via hello `Restart-HcsController` cmdlet innan du tillämpar hello återstående uppdateringar. 
     > 
     > 
2. Upprepa steg 3-5 tooinstall hello LSI drivrutiner och inbyggd programvara snabbkorrigering **KB3186859**. När hello snabbkorrigeringen är installerad, använda hello `Get-HcsSystem` cmdlet. hello LSI versionen ska vara:
   
   * `Lsisas2Version: 2.0.78.00`
3. Upprepa steg 3-5 tooinstall hello Storport och Spaceport uppdatering **KB3121261**.
4. Om du uppdaterar från uppdatering 2 eller en tidigare version, måste du också toodownload:
   
   * iSCSI-lösning med hjälp av KB3146621
   * WMI-lösning med hjälp av KB3103616

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall och kontrollera Underhåll läge snabbkorrigeringar
Använda KB3121899 tooinstall disk firmware-uppdateringar. Dessa är störande uppdateringar och ta runt 30 minuter toocomplete. Du kan välja tooinstall dessa i ett fönster för planerat underhåll av anslutande toohello enhetens seriekonsol.

Observera att om den inbyggda programvaran disken är redan uppdaterade, du inte behöver tooinstall uppdateringarna. Kör hello `Get-HcsUpdateAvailability` cmdlet från hello enhetens seriekonsol toocheck om uppdateringar är tillgängliga och om hello uppdateringar är störande (underhållsläget) eller utan avbrott (standardläget) uppdateringar.

uppdateringar av tooinstall hello disk inbyggd, följ instruktionerna för hello nedan.

1. Placera hello enhet i underhållsläge hello. Observera att du inte bör använda Windows PowerShell-fjärrkommunikation om du ansluter tooa enhet i underhållsläge. I stället köra denna cmdlet på hello enhetskontroll när ansluten via hello enhetens seriekonsol. Ange:
   
    `Enter-HcsMaintenanceMode`
   
    Ett exempel på utdata visas nedan.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
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
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. Övervakaren hello installera förlopp med hjälp av `Get-HcsUpdateStatus` kommando. hello uppdateringen är klar när hello `RunInProgress` ändras för`False`.
4. När hello installationen är klar startar hello styrenhet vilket hello Underhåll läge snabbkorrigeringen har installerats om. Logga in som alternativ 1 med fullständig åtkomst och kontrollera hello disk version på inbyggd programvara. Ange:
   
   `Get-HcsFirmwareVersion`
   
   hello förväntas disk firmware versioner är:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Ett exempel på utdata visas nedan.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    Kör hello `Get-HcsFirmwareVersion` kommandot på hello andra domänkontrollanter tooverify som hello programvaruversionen har uppdaterats. Du kan avsluta hello underhållsläge. toodo så, Skriv följande kommando för varje enhet controller hello:
   
   `Exit-HcsMaintenanceMode`
5. hello domänkontrollanter startas om när du avsluta underhållsläget. Efter hello disk firmware uppdateringar har tillämpats och hello enheten avslutat underhållsläget, returnerar toohello klassiska Azure-portalen. Observera att hello portal inte kanske visar att du installerat hello Underhåll läge uppdateringarna i 24 timmar.

