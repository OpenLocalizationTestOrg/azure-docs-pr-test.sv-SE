<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>tooinstall Underhåll läge uppdateringar via Windows PowerShell för StorSimple
1. Om du inte redan har gjort åt hello enhetens seriekonsol och välj alternativ 1, **logga in med fullständig åtkomst**. 
2. Ange hello lösenord. hello standardlösenordet är **Password1**.
3. I hello kommandotolk, skriver du:
   
     `Get-HcsUpdateAvailability` 
4. Du meddelas om det finns uppdateringar och om hello uppdateringar är störande eller utan avbrott. tooapply störande uppdateringar, behöver du tooput hello enhet i underhållsläge. Se [steg 2: Ange underhållsläge](../articles/storsimple/storsimple-update-device.md#step2) anvisningar.
5. När enheten är i underhållsläge i hello kommandotolk, skriver du:`Start-HcsUpdate`
6. Du uppmanas att bekräfta. När du har bekräftat hello uppdateringar kommer att installeras på hello-domänkontrollant som du använder. När hello uppdateringar har installerats startas hello-styrenhet. 
7. Övervaka hello statusen för uppdateringar. Ange:
   
    `Get-HcsUpdateStatus`
   
    Om hello `RunInProgress` är `True`, hello uppdateringen pågår fortfarande. Om `RunInProgress` är `False`, visar hello uppdateringen har slutförts.  
8. När hello uppdateringen är installerad på hello aktuella domänkontrollanten och den har startats om, och ansluta toohello andra domänkontrollanter och utföra steg 1 till 6.
9. Avsluta underhållsläget när båda domänkontrollanter har uppdaterats. Se [steg 4: avsluta underhållsläget](../articles/storsimple/storsimple-update-device.md#step4) anvisningar.

