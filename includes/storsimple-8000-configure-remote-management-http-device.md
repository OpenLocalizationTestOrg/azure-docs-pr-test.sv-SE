
#### <a name="tooconfigure-remote-management-on-cloud-appliance"></a>tooconfigure fjärrhantering på molnet installation

1. Klicka på **Enheter** i StorSimple Device Manager-tjänsten. Välj och klicka på ditt moln installation hello listan av enheter anslutna toohello tjänst.
    ![Välj StorSimple-molninstallation](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage1.png)

2. Gå för**Inställningar > säkerhet** tooopen hello **säkerhetsinställningar** bladet.

     ![Säkerhetsinställningar för StorSimple](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage2.png)

3. Gå toohello **fjärrhantering** avsnitt. Klicka på rutan **Fjärrhantering**.
     ![StorSimple-fjärrhantering](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage3.png)

4. I hello **fjärrhantering** bladet:

    1. Kontrollera att **Aktivera fjärradministration** är aktiverat.
    2. hello standardvärdet är tooconnect via HTTPS. Du kan välja tooconnect med HTTP. Det är bara acceptabelt att ansluta över HTTP på betrodda nätverk. Kontrollera att HTTP är aktiverat.
    3. Hello kommandofältet hello överst på bladet, klickar du på **... Flera** och klicka sedan på **hämta certifikat** toodownload certifikat för fjärrhantering. Du kan ange en plats i vilka toosave den här filen. Det här certifikatet ska installeras på hello klienten eller värddatorn datorn du använder tooconnect toohello moln installation.

        ![Bladet Fjärrhantering](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage4.png)
5. Klicka på **spara** och när du uppmanas bekräfta hello ändringar.
