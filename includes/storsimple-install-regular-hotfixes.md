<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-regular-hotfixes-via-windows-powershell-for-storsimple"></a>tooinstall reguljära snabbkorrigeringar via Windows PowerShell för StorSimple
1. Ansluta toohello enhetens seriekonsol. Mer information finns i [steg 1: ansluta toohello seriekonsolen](../articles/storsimple/storsimple-update-device.md#step1).
2. Välj alternativ 1, i menyn för seriekonsolen av hello **logga in med fullständig åtkomst**. Ange hello lösenord. hello standardlösenordet är **Password1**.
3. I hello kommandotolk, skriver du:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Detta kommando gäller endast tooregular snabbkorrigeringar. Du kör det här kommandot på en enda domänkontrollant, men både domänkontrollanter kommer att uppdateras.
    > Märker du kanske en domänkontrollant och växling vid fel under uppdateringsprocessen hello; hello redundans påverkar inte filsystemets tillgänglighet eller åtgärden.

4. När du uppmanas ange hello sökvägen toohello delad nätverksmapp som innehåller hello snabbkorrigeringsfiler.
5. Du uppmanas att bekräfta. Typen **Y** tooproceed med hello snabbkorrigeringen installation.

