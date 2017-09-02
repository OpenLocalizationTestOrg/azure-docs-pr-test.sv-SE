
#### <a name="to-configure-remote-management-on-cloud-appliance"></a>Så här konfigurerar du fjärrhantering för molninstallationen

1. Klicka på **Enheter** i StorSimple Device Manager-tjänsten. Markera och klicka på molninstallationen i listan med enheter som är anslutna till tjänsten.
    ![Välj StorSimple-molninstallation](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage1.png)

2. Öppna bladet **Säkerhetsinställningar** genom att gå till **Inställningar > Säkerhet**.

     ![Säkerhetsinställningar för StorSimple](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage2.png)

3. Gå till avsnittet **Fjärrhantering**. Klicka på rutan **Fjärrhantering**.
     ![StorSimple-fjärrhantering](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage3.png)

4. På bladet **Fjärrhantering**:

    1. Kontrollera att **Aktivera fjärradministration** är aktiverat.
    2. Standard är att ansluta via HTTPS. Du kan välja att ansluta med HTTP. Det är bara acceptabelt att ansluta över HTTP på betrodda nätverk. Kontrollera att HTTP är aktiverat.
    3. I kommandofältet längst upp på bladet klickar du på **... Mer** och sedan på **Hämta certifikat** för att hämta ett certifikat för fjärrhantering. Du kan ange var du vill spara filen. Det här certifikatet bör installeras på den klient- eller värddator som du använder för att ansluta till molninstallationen.

        ![Bladet Fjärrhantering](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage4.png)
5. Klicka på **Spara** och bekräfta ändringarna när du uppmanas att göra det.