<!--author=alkohli last changed: 02/22/2016-->


### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure och registrera hello-enhet
1. Komma åt hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol. Se [använda PuTTY tooconnect toohello enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console) anvisningar. **Vara säker på att toofollow hello proceduren exakt eller du inte kan tooaccess hello-konsolen.**
2. Tryck på RETUR en gång tooget Kommandotolken i hello-sessionen som öppnas. 
3. Du kommer att tillfrågas toochoose hello språk som du vill att tooset för din enhet. Ange hello språk och tryck sedan på RETUR. 
   
    ![StorSimple konfigurera och registrera enhet 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)
4. Välj alternativ 1 toolog hello seriekonsolen menyn som visas på med fullständig åtkomst. 
   
    ![StorSimple registrera enhet 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
   
     Slutför steg 5 – 12 tooconfigure hello minsta nödvändiga nätverksinställningarna för enheten. **De här stegen måste toobe utförs på hello aktiva styrenheten för hello enhet.** hello menyn för seriekonsolen indikerar status för hello styrenheten i Banderollmeddelandet hello. Om du inte är ansluten toohello aktiva styrenheten, kopplar du från och Anslut toohello aktiva styrenhet.
5. Skriv in lösenordet i hello kommandotolk. hello enheten standardlösenord är **Password1**.
6. Typen hello följande kommando: `Invoke-HcsSetupWizard`. 
7. En installationsguide kommer visas toohelp som du konfigurerar hello nätverksinställningar för hello enhet. Ange hello hello följande information: 
   
   * IP-adress för hello DATA 0-nätverksgränssnittet
   * Nätmask
   * Gateway
   * IP-adress för primär DNS-server
     
        Observera att hello systemet validerar nätverksinställningarna efter varje steg i processen för hello.
     
     > [!NOTE]
     > Du kan ha toowait några minuter för hello nätmask och hello DNS-inställningar toobe tillämpas. Om du får ett meddelande om ”Kontrollera hello network connectivity tooData 0” Kontrollera hello fysiska nätverksanslutningen på hello DATA 0-nätverksgränssnittet för din aktiva styrenhet.
     > 
     > 
8. (Valfritt) konfigurera din webbproxyserver. Även om webbproxykonfigurationen är valfri, **var medveten om att om du använder en webbproxy så kan du bara konfigurera den här**. Mer information finns för[konfigurera webbproxy för din enhet](../articles/storsimple/storsimple-configure-web-proxy.md).
9. Konfigurera en primär NTP-server för din enhet. NTP-servrar krävs, eftersom din enhet måste synkronisera tiden så att den kan autentisera med molntjänstleverantören. Se till att ditt nätverk tillåter att NTP-trafik toopass från ditt datacenter toohello Internet. Om det inte är möjligt anger du en intern NTP-server. 
10. Av säkerhetsskäl hello enhetens administratörslösenord upphör att gälla efter hello första sessionen och du måste toochange it nu. Ange ett administratörslösenord för enheten när du ombes göra det. Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken. hello lösenordet måste innehålla tre hello följande: gemener, versaler, siffror och särskilda tecken.
    
    <br/>![StorSimple registrera enhet 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)
11. hello sista steget i hello installationsguiden registrerar din enhet med hello StorSimple Manager-tjänsten. För att göra detta måste hello tjänstens registreringsnyckel som du hämtade i steg 2. När du har angett hello registreringsnyckel behöva toowait 2-3 minuter innan hello enheten är registrerad.
    
    > [!NOTE]
    > Du kan trycka på Ctrl + C på alla tid tooexit hello-installationsguiden. Om du har angett alla hello nätverksinställningar (IP-adress för Data 0, nätmask och Gateway) kommer posterna att sparas.
    > 
    > 
    
    ![StorSimple registrera enhet 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)
12. När hello enheten är registrerad visas en krypteringsnyckel. Kopiera den här nyckeln och spara den på säker plats. **Den här nyckeln kommer att krävas med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple Manager-tjänsten.** Se för[StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) mer information om den här nyckeln.
    
    ![StorSimple registrera enhet 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    
    
    > [!NOTE]
    > toocopy hello text från seriekonsolfönstret med hello, bara markera hello text. Sedan ska du kunna toopaste i hello Urklipp eller valfri textredigerare. Använd inte Ctrl + C toocopy hello krypteringsnyckel för tjänstdata. Med Ctrl + C kommer du tooexit hello installationsguiden. Därför kommer inte att ändra hello enhetens administratörslösenord och hello enheten kommer återgå toohello standardlösenordet.
    > 
    > 
13. Avsluta hello seriekonsol.
14. Returnera toohello klassiska Azure-portalen och slutför hello följande steg:
    
    1. Dubbelklicka på din StorSimple Manager service tooaccess hello **Snabbstart** sidan.
    2. Klicka på **Visa anslutna enheter**.
    3. På hello **enheter** kontrollerar hello enheten har lyckats ansluta toohello tjänsten genom att leta upp hello status. hello enhetens status ska vara **Online**.
       
        ![StorSimple-enhetssidan](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
       
        Om hello enhetens status är **Offline**, Vänta några minuter för hello enheten toocome online. 
       
        Om hello enheten fortfarande är offline efter några minuter, måste du till att ditt brandväggsnätverk har konfigurerats enligt beskrivningen i toomake [nätverkskraven för din StorSimple-enhet](../articles/storsimple/storsimple-system-requirements.md). 
       
        Kontrollera att port 9354 är öppen för utgående kommunikation eftersom den används i hello service bus för StorSimple Manager Service-kommunikation till enheten.

