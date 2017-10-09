<!--author=alkohli last changed: 12/01/15-->


#### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure och registrera hello-enhet
1. Komma åt hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol. Se [använda PuTTY tooconnect toohello enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console) anvisningar. **Vara säker på att toofollow hello proceduren exakt eller du inte kan tooaccess hello-konsolen.**
2. Tryck på RETUR en gång tooget Kommandotolken i hello-sessionen som öppnas. 
3. Du kommer att tillfrågas toochoose hello språk som du vill att tooset för din enhet. Ange hello språk och tryck sedan på RETUR. 
   
    ![StorSimple konfigurera och registrera enhet 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)
4. Välj alternativ 1 toolog hello seriekonsolen menyn som visas på med fullständig åtkomst. 
   
    ![StorSimple registrera enhet 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
   
     Slutför steg 5 – 12 tooconfigure hello minsta nödvändiga nätverksinställningarna för enheten. **De här stegen måste toobe utförs på hello aktiva styrenheten för hello enhet.** hello menyn för seriekonsolen indikerar status för hello styrenheten i Banderollmeddelandet hello. Om du inte är ansluten toohello aktiva styrenheten, kopplar du från och Anslut toohello aktiva styrenhet.
5. Skriv in lösenordet i hello kommandotolk. hello enheten standardlösenord är **Password1**.
6. Skriv följande kommando hello:
   
     `Invoke-HcsSetupWizard` 
7. En installationsguide kommer visas toohelp som du konfigurerar hello nätverksinställningar för hello enhet. Ange hello hello följande information: 
   
   * IP-adress för hello DATA 0-nätverksgränssnittet
   * Nätmask
   * Gateway
   * IP-adress för primär DNS-server
   * IP-adress för primär NTP-server
     
     > [!NOTE]
     > Du kan ha toowait några minuter för hello nätmask och hello DNS-inställningar toobe tillämpas. Om du får en ”hello enheten är inte redo”. felmeddelande, kontrollera hello fysiska nätverksanslutningen på hello DATA 0-nätverksgränssnittet för din aktiva styrenhet.
     > 
     > 
8. (Valfritt) konfigurera din webbproxyserver. Även om webbproxykonfigurationen är valfri, **var medveten om att om du använder en webbproxy så kan du bara konfigurera den här**. Mer information finns för[konfigurera webbproxy för din enhet](../articles/storsimple/storsimple-configure-web-proxy.md). Om du stöter på problem under det här steget finns tootroubleshooting vägledning för [fel under webbproxykonfigurationen](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings).

     > [!NOTE]
     > Du kan trycka på Ctrl + C på alla tid tooexit hello-installationsguiden. Alla inställningar som du applicerade innan du anger kommandot kommer att sparas.

1. Av säkerhetsskäl hello enhetens administratörslösenord upphör att gälla efter hello första sessionen och du måste toochange den för efterföljande sessioner. Ange ett administratörslösenord för enheten när du ombes göra det. Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken. hello lösenordet måste innehålla en kombination av gemener, versaler, siffror och specialtecken.
2. hello lösenordet för StorSimple Snapshot Manager ställs också in här. Du använder det här lösenordet när du autentiserar en enhet med din Windowsbaserade dator som kör StorSimple Snapshot Manager. När du uppmanas ange ett lösenord för 14 too15 tecken. hello lösenordet måste innehålla en kombination av tre hello följande: gemener, versaler, siffror och särskilda tecken. 
   
   ![StorSimple registrera enhet 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)
   
   Du kan återställa hello StorSimple Snapshot Manager lösenord från hello StorSimple Managers tjänstgränssnitt. Detaljerade anvisningar finns för[ändra hello StorSimple lösenord med hello StorSimple Manager-tjänsten](../articles/storsimple/storsimple-change-passwords.md).
   
   tootroubleshoot eventuella problem i det här steget finns tootroubleshooting vägledning för [fel relaterade toopasswords](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords).
3. hello sista steget i hello installationsguiden registrerar din enhet med hello StorSimple Manager-tjänsten. För att göra detta måste hello tjänstens registreringsnyckel som du hämtade i steg 2. När du har angett hello registreringsnyckel behöva toowait 2-3 minuter innan hello enheten är registrerad.
   
   tootroubleshoot eventuella fel med enhetsregistrering, referera för[fel vid enhetsregistrering](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-device-registration). För detaljerad felsökning kan du också läsa för[stegvis Felsökningsexempel](../articles/storsimple/storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example).
4. När hello enheten är registrerad visas en krypteringsnyckel. Kopiera den här nyckeln och spara den på säker plats.
   
   > [!WARNING]
   > Den här nyckeln kommer att krävas med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple Manager-tjänsten. Se för[StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) mer information om den här nyckeln.
   > 
   > 
   
    ![StorSimple registrera enhet 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)
   
    toocopy hello text från seriekonsolfönstret med hello, bara markera hello text. Sedan ska du kunna toopaste i hello Urklipp eller valfri textredigerare. Använd inte Ctrl + C toocopy hello krypteringsnyckel för tjänstdata. Med Ctrl + C kommer du tooexit hello installationsguiden. Därför hello enhetens administratörslösenord och lösenordet för StorSimple Snapshot Manager av hello ändras inte och hello enheten kommer återgå toohello standardlösenordet.
5. Avsluta hello seriekonsol.
6. Returnera toohello klassiska Azure-portalen och slutför hello följande steg:
   
   1. Dubbelklicka på din StorSimple Manager service tooaccess hello **Snabbstart** sidan.
   2. Klicka på **Visa anslutna enheter**.
   3. På hello **enheter** kontrollerar hello enheten har lyckats ansluta toohello tjänsten genom att leta upp hello status. hello enhetens status ska vara **Online**. Om hello enhetens status är **Offline**, Vänta några minuter för hello enheten toocome online.
   
   ![StorSimple-enhetssidan](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
   
   > [!IMPORTANT]
   > När hello enheten är online, Anslut hello nätverkskablarna som du hade dragit ut i hello början av det här steget.
   > 
   > 

När hello enheten har registrerats och inte är online, kan du köra hello `Test-HcsmConnection -Verbose` tooensure som hello nätverksanslutningen är felfri. Hello detaljerad användning av denna cmdlet finns för[cmdlet-referens för Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx).

![Video tillgänglig](./media/storsimple-configure-and-register-device/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur tooconfigure och registrera enheten via Windows PowerShell för StorSimple, klickar du på [här](https://azure.microsoft.com/documentation/videos/initialize-the-storsimple-appliance/).

