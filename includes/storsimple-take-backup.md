<!--author=alkohli last changed: 9/17/15-->

### <a name="tootake-a-backup"></a>tootake en säkerhetskopia
1. På hello enhet **Snabbstart** klickar du på **lägga till en princip för säkerhetskopiering**. Detta startar guiden för hello Lägg till princip för säkerhetskopiering. 
2. På hello **definiera din princip för säkerhetskopiering** sidan:
   
   1. Ange ett namn som innehåller mellan 3 och 150 tecken för din princip för säkerhetskopiering.
   2. Välj hello volymer toobe säkerhetskopieras. Om du väljer fler än en volym, kommer volymerna att grupperade tillsammans toocreate en kraschkonsekvent säkerhetskopia.
   3. Klicka på pilikonen hello ![pilikon](./media/storsimple-take-backup/HCS_ArrowIcon-include.png). 
      
      ![Lägg till princip för säkerhetskopiering](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard1M-include.png)
3. På hello **definiera ett schema** sidan:
   
   1. Välj hello typ av säkerhetskopiering hello nedrullningsbara listan. Välj **Lokal ögonblicksbild**, för snabbare återställningar. Välj **Ögonblicksbild i molnet**, för dataåterhämtning.
   2. Ange hello frekvens för säkerhetskopiering i minuter, timmar, dagar eller veckor.
   3. Välj en kvarhållningstid. Hej kvarhållningsalternativen beror på hello frekvens för säkerhetskopiering. Till exempel för en daglig princip kan hello kvarhållning anges i veckor, medan kvarhållningen för en månatlig princip är i månader.
   4. Välj hello och-datum för hello säkerhetskopieringsprincip.
   5. Välj hello **aktivera** kryssrutan tooenable hello säkerhetskopieringsprincip. 
   6. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-take-backup/HCS_CheckIcon-include.png) toosave hello princip.
      
      ![Lägg till princip för säkerhetskopiering](./media/storsimple-take-backup/HCS_AddBackupPolicyWizard2M-include.png)
      
      Du har nu en princip för säkerhetskopiering som skapar schemalagda säkerhetskopieringar av din volymdata.

Du har slutfört hello enhetskonfigurationen. 

![Video tillgänglig](./media/storsimple-take-backup/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur tootake en StorSimple-säkerhetskopia, klickar du på [här](https://azure.microsoft.com/documentation/videos/take-a-storsimple-backup/).

