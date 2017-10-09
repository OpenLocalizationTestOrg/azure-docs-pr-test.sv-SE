#### <a name="toostop-and-start-a-cloud-appliance"></a>toostop och starta en moln-installation

1. toostop en moln-enhet går toohello VM för din enhet i molnet.
    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)

2. Hello kommandofältet klickar du på **stoppa**.

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. Klicka på **Ja** när du uppmanas att bekräfta åtgärden.

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. När du stoppar en virtuell dator avallokeras den. Hello molnet installation stoppas dess status är **Deallocating**. När hello molnet installation har avbrutits dess status är **Stoppad (frigjord)**.

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. När en virtuell dator har stoppats, klickar du på **starta** (knappen blir tillgänglig) toostart hello VM. När hello molnet enheten har startats dess status är **igång**.

    ![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

Använd följande cmdlet: ar toostop hello och starta en moln-installation.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-cloud-appliance"></a>toorestart en moln-installation

toorestart en moln-enhet går toohello VM för din enhet i molnet. Hello kommandofältet klickar du på **starta om**. När du uppmanas bekräfta hello omstart. När hello molnet installation är klar för du toouse, är dess status **kör**.

![Virtuell dator i StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

Använd hello följande cmdlet toorestart en moln-installation.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

