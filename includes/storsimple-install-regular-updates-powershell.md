<!--author=SharS last changed: 11/18/16-->

#### <a name="tooinstall-regular-updates-via-windows-powershell-for-storsimple"></a>tooinstall regelbundna uppdateringar via Windows PowerShell för StorSimple
1. Öppna hello enhetens seriekonsol och välj alternativ 1, **logga in med fullständig åtkomst**. Ange hello lösenord. hello standardlösenordet är *Password1*. 
2. I hello kommandotolk, skriver du:
   
     `Get-HcsUpdateAvailability`
   
    Du meddelas om det finns uppdateringar och om hello uppdateringar är störande eller utan avbrott.
3. I hello kommandotolk, skriver du:
   
     `Start-HcsUpdate`
   
    hello uppdateringen startar.

> [!IMPORTANT]
> * Detta kommando gäller endast tooregular uppdateringar. Du kör det här kommandot på en enda domänkontrollant, men både domänkontrollanter kommer att uppdateras. 
> * Märker du kanske en domänkontrollant och växling vid fel under uppdateringsprocessen hello; hello redundans påverkar inte filsystemets tillgänglighet eller åtgärden.
> 
> 

