<!--author=SharS last changed: 02/22/2016-->

### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure och registrera hello-enhet
1. Komma åt hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol. Se [använda PuTTY tooconnect toohello enhetens seriekonsol](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) anvisningar. **Vara säker på att toofollow hello proceduren exakt eller du inte kan tooaccess hello-konsolen.**
2. Tryck på RETUR en gång tooget Kommandotolken i hello-sessionen som öppnas.
3. Du kommer att tillfrågas toochoose hello språk som du vill att tooset för din enhet. Ange hello språk och tryck sedan på RETUR.
   
    ![StorSimple konfigurera och registrera enhet 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. Välj alternativ 1 toolog hello seriekonsolen menyn som visas på med fullständig åtkomst.
   
    ![StorSimple registrera enhet 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. Utföra hello följande steg tooconfigure hello minsta nödvändiga nätverksinställningarna för enheten.
   
   > [!IMPORTANT]
   > De här stegen måste toobe utförs på hello aktiva styrenheten för hello enhet. hello menyn för seriekonsolen indikerar status för hello styrenheten i Banderollmeddelandet hello. Om du inte ansluta toohello aktiva styrenheten koppla från och ansluta toohello aktiva styrenhet.
   > 
   > 
   
   1. Skriv in lösenordet i hello kommandotolk. hello enheten standardlösenord är **Password1**.
   2. Skriv följande kommando hello:
      
        `Invoke-HcsSetupWizard`
   3. En installationsguide kommer visas toohelp som du konfigurerar hello nätverksinställningar för hello enhet. Ange hello följande information:
      
      * IP-adress för DATA 0-nätverksgränssnittet
      * Nätmask
      * Gateway
      * IP-adress för primär DNS-server
      * IP-adress för primär NTP-server
      
      > [!NOTE]
      > Du kan ha toowait några minuter för hello nätmask och DNS-inställningar toobe tillämpas.
      > 
      > 
   4. Alternativt kan du konfigurera din webbproxyserver.
      
      > [!IMPORTANT]
      > Även om webbproxykonfigurationen är valfri, Tänk på att om du använder en webbproxy, du kan bara konfigurera den här. Mer information finns för[konfigurera webbproxy för din enhet](../articles/storsimple/storsimple-configure-web-proxy.md).
      > 
      > 
6. Tryck på Ctrl + C tooexit hello-installationsguiden.
7. Installera hello uppdateringar på följande sätt:
   
   1. Använd följande cmdlet tooset IP-adresser på båda hello domänkontrollanter hello:
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. Kommandotolken hello kör `Get-HcsUpdateAvailability`. Du får ett meddelande att uppdateringar är tillgängliga.
   3. Kör `Start-HcsUpdate`. Du kan köra det här kommandot på en nod. Uppdateringarna tillämpas på hello första domänkontrollant, växlar hello domänkontrollant och sedan hello hello uppdateringarna tillämpas på andra domänkontrollanter.
      
      Du kan övervaka hello hello uppdateringen genom att köra `Get-HcsUpdateStatus`.    
      
      hello visar följande exempel på utdata hello uppdatering pågår.
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ````
      
      följande exempel på utdata hello anger hello uppdateringen är klar.
      
      ```
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ```
      
      Det kan ta upp too11 timmar tooapply alla hello uppdateringar, inklusive hello Windows-uppdateringar.
8. Kör hello följande cmdlet toopoint hello enheten toohello Microsoft Azure Government portalen (eftersom den pekar toohello offentliga klassiska Azure-portalen som standard). Både domänkontrollanter kommer att startas om. Vi rekommenderar att du använder två PuTTY sessioner toosimultaneously ansluta tooboth domänkontrollanterna så att du kan se när varje styrenhet har startats om.
   
    `Set-CloudPlatform -AzureGovt_US`
   
   Visas ett bekräftelsemeddelande. Acceptera standardvärdet för hello (**Y**).
9. Kör följande cmdlet tooresume installationsprogrammet hello:
   
    `Invoke-HcsSetupWizard`
   
    ![Återuppta installationsguiden](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
   När du fortsätter installationen är hello guiden hello uppdatering 2-version.
10. Acceptera hello nätverksinställningar. Ett verifieringsmeddelande visas när du har accepterat varje inställning.
11. Av säkerhetsskäl hello enhetens administratörslösenord upphör att gälla efter hello första sessionen och du måste toochange it nu. Ange ett administratörslösenord för enheten när du ombes göra det. Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken. hello lösenordet måste innehålla tre hello följande: gemener, versaler, siffror och särskilda tecken.
    
    <br/>![StorSimple registrera enhet 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. hello sista steget i hello installationsguiden registrerar din enhet med hello StorSimple Manager-tjänsten. För detta måste du kommer måste hello tjänstens registreringsnyckel som du fick i [steg 2: hämta hello nyckel för tjänstregistrering](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key). När du har angett hello registreringsnyckel behöva toowait 2-3 minuter innan hello enheten är registrerad.
    
    > [!NOTE]
    > Du kan trycka på Ctrl + C på alla tid tooexit hello-installationsguiden. Om du har angett alla hello nätverksinställningar (IP-adress för Data 0, nätmask och Gateway) kommer posterna att sparas.
    > 
    > 
    
    ![StorSimple registrering pågår](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. När hello enheten är registrerad visas en krypteringsnyckel. Kopiera den här nyckeln och spara den på säker plats. **Den här nyckeln kommer att krävas med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple Manager-tjänsten.** Se för[StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) mer information om den här nyckeln.
    
    ![StorSimple registrera enhet 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > toocopy hello text från seriekonsolfönstret med hello, bara markera hello text. Sedan ska du kunna toopaste i hello Urklipp eller valfri textredigerare.
    > 
    > Använd inte Ctrl + C toocopy hello krypteringsnyckel för tjänstdata. Med Ctrl + C kommer du tooexit hello installationsguiden. Därför kommer inte att ändra hello enhetens administratörslösenord och hello enheten kommer återgå toohello standardlösenordet.
    > 
    > 
14. Avsluta hello seriekonsol.
15. Returnera toohello Azure Government-portalen och slutför hello följande steg:
    
    1. Dubbelklicka på din StorSimple Manager service tooaccess hello **Snabbstart** sidan.
    2. Klicka på **Visa anslutna enheter**.
    3. På hello **enheter** kontrollerar hello enheten har lyckats ansluta toohello tjänsten genom att leta upp hello status. hello enhetens status ska vara **Online**.
       
        ![StorSimple-enhetssidan](./media/storsimple-configure-and-register-device-gov-u2/HCS_DeviceOnline-gov-include.png)
       
        Om hello enhetens status är **Offline**, Vänta några minuter för hello enheten toocome online.
       
        Om hello enheten fortfarande är offline efter några minuter, måste du till att ditt brandväggsnätverk har konfigurerats enligt beskrivningen i toomake [nätverkskraven för din StorSimple-enhet](../articles/storsimple/storsimple-system-requirements.md).
       
        Kontrollera att port 9354 är öppen för utgående kommunikation eftersom den används i hello service bus för StorSimple Manager Service-kommunikation till enheten.

