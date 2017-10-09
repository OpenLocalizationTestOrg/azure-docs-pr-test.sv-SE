<!--author=alkohli last changed: 01/18/2017-->


#### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure och registrera hello-enhet

1. Komma åt hello Windows PowerShell-gränssnittet på din StorSimple enhetens seriekonsol. Se [använda PuTTY tooconnect toohello enhetens seriekonsol](#use-putty-to-connect-to-the-device-serial-console) anvisningar. **Vara säker på att toofollow hello proceduren exakt eller du inte kan tooaccess hello-konsolen.**

2. Hello-sessionen som öppnas, trycker du på **RETUR** en gång tooget en kommandotolk.

3. Du kommer att tillfrågas toochoose hello språk som du vill att tooset för din enhet. Ange hello språk och tryck sedan på **RETUR**.

4. Hej seriekonsolen menyn som visas väljer du alternativ 1 för**logga in med fullständig åtkomst**.
     Slutför steg 5 – 12 tooconfigure hello minsta nödvändiga nätverksinställningarna för enheten. **De här stegen måste toobe utförs på hello aktiva styrenheten för hello enhet.** hello menyn för seriekonsolen indikerar status för hello styrenheten i Banderollmeddelandet hello. Om du inte är ansluten toohello aktiva styrenheten, kopplar du från och Anslut toohello aktiva styrenhet.

5. Skriv in lösenordet i hello kommandotolk. hello enheten standardlösenord är **Password1**.

6. Typen hello följande kommando: `Invoke-HcsSetupWizard`.

7. En installationsguide kommer visas toohelp som du konfigurerar hello nätverksinställningar för hello enhet. Ange hello hello följande information:
   
   * IP-adress för hello DATA 0-nätverksgränssnittet
   * Nätmask
   * Gateway
   * IP-adress för primär DNS-server

   Ett exempel på de utdata som returneras visas nedan.

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Active
        ---------------------------------------------------------------

        Your device needs toobe registered with hello Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' tooset up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like tooconfigure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    I föregående exempel på utdata hello, ser du att hello systemet validerar nätverksinställningarna efter varje steg i processen för hello.

     > [!NOTE]
     > Du kan ha toowait några minuter för hello nätmask och hello DNS-inställningar toobe tillämpas. Om du får ett meddelande om ”Kontrollera hello network connectivity tooData 0” Kontrollera hello fysiska nätverksanslutningen på hello DATA 0-nätverksgränssnittet för din aktiva styrenhet.

8. (Valfritt) konfigurera din webbproxyserver. Även om webbproxykonfigurationen är valfri, **var medveten om att om du använder en webbproxy så kan du bara konfigurera den här**. Mer information finns för[konfigurera webbproxy för din enhet](../articles/storsimple/storsimple-8000-configure-web-proxy.md).
9. Konfigurera en primär NTP-server för din enhet. NTP-servrar krävs, eftersom din enhet måste synkronisera tiden så att den kan autentisera med molntjänstleverantören. Se till att ditt nätverk tillåter att NTP-trafik toopass från ditt datacenter toohello Internet. Om det inte är möjligt anger du en intern NTP-server.

    Ett exempel på utdata visas nedan.

    ```
        Would you like tooconfigure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. Av säkerhetsskäl hello enhetens administratörslösenord upphör att gälla efter hello första sessionen och du måste toochange it nu. Ange ett administratörslösenord för enheten när du ombes göra det. Ett giltigt enhetsadministratörslösenord för enheten måste vara mellan 8 och 15 tecken. hello lösenordet måste innehålla tre hello följande: gemener, versaler, siffror och särskilda tecken.

    ```
        hello device administrator password must be between 8 and 15 characters. hello password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. hello sista steget i installationsguiden för hello registrerar enheten med hello StorSimple enheten Manager-tjänsten. För att göra detta måste hello tjänstens registreringsnyckel som du hämtade i steg 2. När du har angett hello registreringsnyckel behöva toowait 2-3 minuter innan hello enheten är registrerad.
    
    > [!NOTE]
    > Du kan trycka på Ctrl + C på alla tid tooexit hello-installationsguiden. Om du har angett alla hello nätverksinställningar (IP-adress för Data 0, nätmask och Gateway) kommer posterna att sparas.
    
    Ett exempel på utdata visas nedan.

    ```
        hello service registration key is available in hello StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. När hello enheten är registrerad visas en krypteringsnyckel. Kopiera den här nyckeln och spara den på säker plats. **Den här nyckeln kommer att krävas med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple enheten Manager-tjänsten.** Se för[StorSimple-säkerhet](../articles/storsimple/storsimple-security.md) mer information om den här nyckeln.
    
    ![StorSimple registrera enhet 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > toocopy hello text från seriekonsolfönstret med hello, bara markera hello text. Sedan ska du kunna toopaste i hello Urklipp eller valfri textredigerare. Använd inte Ctrl + C toocopy hello krypteringsnyckel för tjänstdata. Med Ctrl + C kommer du tooexit hello installationsguiden. Därför kommer inte att ändra hello enhetens administratörslösenord och hello enheten kommer återgå toohello standardlösenordet.
    
13. Avsluta hello seriekonsol.
14. Returnera toohello Azure-portalen och slutför hello följande steg:
    
    1. Gå tooyour StorSimple enheten Manager-tjänsten.
    2. Klicka på **Enheter**.
    3. Kontrollera i hello tabular lista över enheter, hello enheten har lyckats ansluta toohello tjänsten genom att leta upp hello status. hello enhetens status ska vara **klar tooset in**.
       
        ![StorSimple-enhetssidan](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        Du kan behöva toowait för ett par minuter för hello enhetens status toochange för**klar tooset in**.
       
        Om hello enheten inte visas i listan, måste du till att ditt brandväggsnätverk har konfigurerats enligt beskrivningen i toomake [nätverkskraven för din StorSimple-enhet](../articles/storsimple/storsimple-8000-system-requirements.md). Kontrollera att port 9354 är öppen för utgående kommunikation eftersom den används i hello service bus för kommunikation för StorSimple Device Manager-tjänsten till enheten.

