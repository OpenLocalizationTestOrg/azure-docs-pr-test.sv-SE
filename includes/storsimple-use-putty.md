<!--author=SharS last changed: 9/17/15-->

#### <a name="tooconnect-through-hello-serial-console"></a>tooconnect via hello seriekonsol
1. Anslut den seriella kabel toohello enheten (direkt eller via en USB-Serieadapter).
2. Öppna hello **Kontrollpanelen**, och sedan öppna hello **Enhetshanteraren**.
3. Identifiera hello COM-port som visas i följande illustration hello.
   
     ![Anslut via seriekonsol](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Starta PuTTY. 
5. I högra fönstret hello ändra hello **anslutningstypen** för**seriella**.
6. Ange hello lämplig COM-port i hello högra rutan. Kontrollera att hello seriekonfigurationen är inställda på följande sätt:
   
   * Hastighet: 115 200
   * Databitar: 8
   * Stoppbitar: 1
   * Paritet: ingen
   * Flödeskontroll: ingen
     
     De här inställningarna visas i följande illustration hello.
     
     ![PuTTY-inställningar](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Om hello standardinställningen för flödeskontroll inte fungerar, försök med hello flödet kontrollen tooXON/XOFF.
     > 
     > 
7. Klicka på **öppna** toostart en seriesession.

