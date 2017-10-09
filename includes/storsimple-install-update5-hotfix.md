<!--author=alkohli last changed: 08/21/17-->

#### <a name="toodownload-hotfixes"></a>toodownload snabbkorrigeringar

Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.

1. Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.

    ![Installera katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload, till exempel **4037264**, och klicka sedan på **Sök**.
   
    hello snabbkorrigeringen visas, till exempel **kumulativa programvara paket Update 5.0 för StorSimple 8000-serien**.
   
    ![Sökkatalog](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. Klicka på **Hämta**. Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear. Klicka på hello filer toodownload toohello den angivna platsen och mapp. hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.
5. Sök efter eventuella ytterligare snabbkorrigeringar som anges i hello tabellen ovan (**4037266**), och ladda ned hello motsvarande filer toohello specifika mappar som anges i föregående tabell hello.

> [!NOTE]
> hello snabbkorrigeringar måste vara tillgänglig från båda domänkontrollanterna toodetect eventuella eventuella felmeddelanden från hello peer-domänkontrollant.
>
> hello snabbkorrigeringar måste kopieras i 3 separata mappar. Till exempel hello enheten MDS-program/TIS agentuppdatering kan kopieras i _FirstOrderUpdate_ mapp, alla hello andra utan avbrott uppdateringar kan kopieras i hello _SecondOrderUpdate_ mapp, och Underhåll läge uppdateringar kopierade i _ThirdOrderUpdate_ mapp.

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall och kontrollera standardläget snabbkorrigeringar

Utför följande steg tooinstall hello och kontrollera snabbkorrigeringar regular-läge. Om du redan installerat med hello Azure-portalen kan du gå vidare för[installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello snabbkorrigeringar, åtkomst hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol. Följ hello detaljerade instruktioner i [använda PuTTy tooconnect toohello seriekonsolen](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). Hello Kommandotolken, tryck på **RETUR**.
2. Välj **alternativ 1** toolog på toohello enhet med fullständig åtkomst. Vi rekommenderar att du installerar snabbkorrigeringen hello på hello passiva domänkontrollant först.
3. snabbkorrigering för tooinstall hello, Kommandotolken hello typ:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Använd IP i stället för DNS på resurssökvägen i hello ovanstående kommando. hello autentiseringsuppgiftsparametern används bara om du ansluter till en autentiserad resurs.
   
    Vi rekommenderar att du använder hello autentiseringsuppgifter parametern tooaccess resurser. Även resurser som är öppna för ”alla” är vanligtvis inte att öppna toounauthenticated användare.
   
4. Ange hello lösenord när du uppmanas. Ett exempel på utdata för att installera hello första uppdateringar visas nedan. För hello första uppdatering behöver du toopoint toohello specifik fil.

    >[!NOTE] 
    > Du bör installera hello _HcsSoftwareUpdate.exe_ första. När installationen har slutförts kan du sedan installera _CisMdsAgentUpdate.exe_.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
5. Typen **Y** när begärd tooconfirm hello snabbkorrigeringsinstallationen.
6. Övervaka hello uppdateringen med hjälp av hello `Get-HcsUpdateStatus` cmdlet. hello-update först slutför på hello passiva domänkontrollant. När hello passiva controller har uppdaterats, finns en växling vid fel och hello uppdatering används sedan på hello andra styrenheten. hello uppdateringen är klar när båda hello domänkontrollanter uppdateras.
   
    hello visar följande exempel på utdata hello uppdatering pågår. Hej `RunInprogress` är `True` när hello uppdatering pågår.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     följande exempel på utdata hello anger hello uppdateringen är klar. Hej `RunInProgress` är `False` när hello uppdateringen är klar.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > Ibland kan hello cmdlet rapporter `False` när hello uppdateringen pågår fortfarande. tooensure som hello snabbkorrigeringen är klar, Vänta några minuter, kör kommandot och kontrollera att hello `RunInProgress` är `False`. Om det är har hello snabbkorrigeringen slutförts.

7. Kontrollera hello system programvaruversioner när hello programuppdateringen har slutförts. Ange:
   
    `Get-HcsSystem`
   
    Du bör se hello följande versioner:
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   *  `HcsSoftwareVersion: 6.3.9600.17845`
   
    Om hello versionsnumret inte ändras efter att de hello uppdateringen anger hello snabbkorrigeringen har misslyckats tooapply. Kontakta [Microsoft Support](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) för ytterligare hjälp om du ser det här.
     
    > [!IMPORTANT]
    > Du måste starta om hello aktiva styrenheten via hello `Restart-HcsController` cmdlet innan du tillämpar hello nästa uppdatering.
     
8. Upprepa steg 3-6 tooinstall hello _CisMDSAgentupdate.exe_ agent hämtas tooyour _FirstOrderUpdate_ mapp.
8. Upprepa steg 3 – 6 tooinstall hello andra uppdateringar. 

    > [!NOTE] 
    > För andra uppdateringar, flera uppdateringar installeras genom att bara köra hello `Start-HcsHotfix cmdlet` och peka toohello mappen där det finns andra uppdateringar. hello-cmdlet utför alla hello tillgängliga uppdateringar i hello mapp. Om en uppdatering redan är installerad, identifierar som hello update logik och inte tillämpa uppdateringen.

    När du har installerat alla hello snabbkorrigeringar använder hello `Get-HcsSystem` cmdlet. hello versioner bör vara:
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall och kontrollera Underhåll läge snabbkorrigeringar

Använda KB4037263 tooinstall disk firmware-uppdateringar. Dessa är störande uppdateringar och ta runt 30 minuter toocomplete. Du kan välja tooinstall dessa i ett fönster för planerat underhåll av anslutande toohello enhetens seriekonsol.

> [!NOTE] 
> Om den inbyggda programvaran disken är redan uppdaterade, behöver du inte tooinstall uppdateringarna. Kör hello `Get-HcsUpdateAvailability` cmdlet från hello enhetens seriekonsol toocheck om uppdateringar är tillgängliga och om hello uppdateringar är störande (underhållsläget) eller utan avbrott (standardläget) uppdateringar.

uppdateringar av tooinstall hello disk inbyggd, följ instruktionerna för hello nedan.

1. Placera hello enhet i underhållsläge hello. 

    > [!NOTE] 
    > Använd inte fjärrkommunikation med Windows PowerShell om du ansluter tooa enhet i underhållsläge. I stället köra denna cmdlet på hello enhetskontroll när ansluten via hello enhetens seriekonsol.

    tooplace hello domänkontrollant i underhållsläge, skriver du:
   
    `Enter-HcsMaintenanceMode`
   
    Ett exempel på utdata visas nedan.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
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
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
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
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   Ett exempel på utdata visas nedan.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
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
   
    Kör hello `Get-HcsFirmwareVersion` kommandot på hello andra domänkontrollanter tooverify som hello programvaruversionen har uppdaterats. Du kan avsluta hello underhållsläge. toodo så, Skriv följande kommando för varje enhet controller hello:
   
   `Exit-HcsMaintenanceMode`

5. hello domänkontrollanter startas om när du avsluta underhållsläget. Efter hello disk firmware uppdateringar har tillämpats och hello enheten avslutat underhållsläget, returnerar toohello Azure-portalen. Observera att hello portal inte kanske visar att du installerat hello Underhåll läge uppdateringarna i 24 timmar.

