<!--author=alkohli last changed: 01/12/17-->

### <a name="tootake-a-backup"></a>tootake en säkerhetskopia

1. Gå tooyour StorSimple enheten Manager-tjänsten. Hello tabular lista över enheter, markera och klicka på enheten och klicka sedan på **alla inställningar**. I hello **inställningar** bladet går för**Inställningar > Hantera > Säkerhetskopiera princip**.

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu1.png)

2. I hello **säkerhetskopiera princip** bladet, klickar du på **+ Lägg till princip**.

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu2.png)

3. I hello **skapa säkerhetskopieringsprincip** bladet, ange ett namn som innehåller mellan 3 och 150 tecken för din princip för säkerhetskopiering.

4. Välj hello volymer toobe säkerhetskopieras. Om du väljer fler än en volym är volymerna grupperade tillsammans toocreate en kraschkonsekvent säkerhetskopia.

    ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu4.png)

5. På bladet **Lägg till första schema**:

    1. Välj hello typ av säkerhetskopiering. För snabbare återställningar väljer du **Lokal ögonblicksbild**. För dataåterhämtning väljer du ögonblicksbilden av **molndata**.
    2. Ange hello frekvens för säkerhetskopiering i minuter, timmar, dagar eller veckor.
    3. Välj en kvarhållningstid. Hej kvarhållningsalternativen beror på hello frekvens för säkerhetskopiering. Till exempel för en daglig princip kan hello kvarhållning anges i veckor, medan kvarhållningen för en månatlig princip är i månader.
    4. Välj hello och-datum för hello säkerhetskopieringsprincip.
    5. Klicka på **OK** toocreate hello säkerhetskopieringsprincip.

        ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. Klicka på **skapa** toostart hello säkerhetskopieringsprincip skapas. Du meddelas när hello säkerhetskopieringsprincip har skapats. hello lista över principer för säkerhetskopiering har även uppdaterats.
      
      ![Lägg till princip för säkerhetskopiering](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      Nu har du en säkerhetskopieringspolicy som skapar schemalagda säkerhetskopieringar av din volymdata.




