<!--author=alkohli last changed: 08/04/17-->

#### <a name="tooinstall-an-update-from-hello-azure-portal"></a>tooinstall en uppdatering från hello Azure-portalen

1. Välj enheten hello StorSimple-tjänsten på sidan.

    ![Välj enhet](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. Navigera för**Enhetsinställningar** > **enhetsuppdateringar**.

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. Ett meddelande visas om det finns nya uppdateringar. Du kan också i hello **enhetsuppdateringar** bladet, klickar du på **Sök efter uppdateringar**. Ett jobb skapas tooscan efter uppdateringar. Du meddelas när hello jobbet slutförs ordentligt.

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. Vi rekommenderar att du läser hello viktig information innan du installerar en uppdatering på enheten. tooapply uppdateringar, klickar du på **installera uppdateringar**. I hello **bekräfta regelbundna uppdateringar** bladet granska hello krav toocomplete innan du installerar uppdateringar. Markera kryssrutan hello tooindicate att du är klar tooupdate hello enheten och klicka sedan på **installera**.

    ![Klicka på uppdatering av enheter](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. En uppsättning nödvändiga kontrollerar startar. Dessa kontroller omfattar följande:
   
   * **Domänkontrollanten hälsokontroller** tooverify både hello styrenheter är felfri och online.
   * **Maskinvara komponenten hälsokontroller** tooverify alla hello maskinvarukomponenter på StorSimple-enheten är felfria.
   * **DATA 0 kontrollerar** tooverify att DATA 0 är aktiverad på enheten. Om inte det här gränssnittet är aktiverat måste du aktivera det och sedan försöka igen.

    hello uppdatering hämtas och installeras bara om alla hello kontroller har slutförts. Du meddelas när hello kontrollerar pågår. Om hello prechecks misslyckas sedan anges du med hello orsakerna till felet. Åtgärda problemen och försök sedan hello igen. Du kanske måste toocontact Microsoft-supporten om du kan lösa dessa problem själv.

7. När hello prechecks har slutförts, skapas ett uppdateringsjobb. Du meddelas när hello Uppdateringsjobbet har skapats.
   
    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    hello uppdateringen tillämpas sedan på enheten.

9. Det tar några timmar toocomplete hello uppdatering. Välj hello Uppdateringsjobbet och klicka på **information** tooview hello informationen om hello jobb när som helst.

    ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update5-via-portal/update8.png)

     Du kan också övervaka hello hello Uppdateringsjobbet från **Enhetsinställningar > jobb**. På hello **jobb** bladet kan du se hello uppdatera status.

     ![Uppdatera skapande av jobb](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. När hello jobbet har slutförts, går toohello **Enhetsinställningar > enhetsuppdateringar**. hello programvaruversionen ska nu uppdateras.

