<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>tooinstall Underhåll läge snabbkorrigeringar via Windows PowerShell för StorSimple
> [!IMPORTANT]
> I underhållsläge, du måste först tooapply hello snabbkorrigeringen på en domänkontrollant och sedan hello andra domänkontrollanter på.
> 
> 

1. Placera hello enhet i underhållsläge. Se [steg 2: Ange underhållsläge](../articles/storsimple/storsimple-update-device.md#step2) anvisningar för hur tooenter underhållsläge.
2. tooapply hello snabbkorrigering, typ:
   
     `Start-HcsHotfix` 
3. När du uppmanas ange hello sökvägen toohello delad nätverksmapp som innehåller hello snabbkorrigeringsfiler.
4. Du uppmanas att bekräfta. Typen **Y** tooproceed med hello snabbkorrigeringen installation.
5. Efter installation av hello snabbkorrigeringen på en domänkontrollant, logga in toohello andra styrenheten. Snabbkorrigeringen hello som du gjorde för tidigare hello-styrenhet.
6. Avsluta underhållsläget när hello snabbkorrigeringarna tillämpas. Se [steg 4: avsluta underhållsläget](../articles/storsimple/storsimple-update-device.md#step4) anvisningar.

