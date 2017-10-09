<!--author=SharS last changed: 02/04/2016-->

#### <a name="toocreate-a-volume"></a>toocreate en volym
1. På hello enhet **Snabbstart** klickar du på **lägga till en volym**. Detta startar hello guiden Lägg till en volym.
2. I hello guiden Lägg till en volym, under **grundläggande inställningar**, hello följande:
   
   1. Ange ett **namn** för volymen.
   2. Ange hello **Etableringskapacitet** för volymen i GB eller TB. hello volymens kapacitet måste vara mellan 1 GB och 64 TB för en fysisk enhet.
   3. Välj hello på hello nedrullningsbara listan **användningstyp** för volymen. 
   4. Om du använder volymen för arkiveringsdata, markerar du hello **Använd volymen för mindre ofta använda arkiveringsdata** kryssrutan. För alla andra användningsfall, väljer du helt enkelt **Nivåindelad volym**. (Nivåindelade volymer kallades förut för primära volymer).
      
        ![Lägg till volym](./media/storsimple-create-volume/ScreenshotUpdate1VolumeFlow.png)
      
      1. Klicka på pilikonen hello ![pilikon](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) toogo toohello nästa sida.
3. I hello **ytterligare inställningar** dialogrutan Lägg till en ny åtkomstkontrollpost (ACR):
   
   1. Ange ett **namn** för din ACR.
   2. Under **iSCSI-Initierarnamn**, ange hello iSCSI kvalificerade namn (IQN) för Windows-värd. Om du inte har hello IQN, går för[Get hello IQN för en Windows Server-värd](#get-the-iqn-of-a-windows-server-host).
   3. Vi rekommenderar att du aktiverar en standard säkerhetskopiering genom att välja hello **aktiverar en standard säkerhetskopiering för den här volymen** kryssrutan. hello standard säkerhetskopieringen skapar en princip som körs klockan 22.30 varje dag (enhetens tid) och skapar en ögonblicksbild av den här volymen i molnet.
      
      > [!NOTE]
      > När hello säkerhetskopieringen har aktiverats här, kan den inte återställas. Du behöver tooedit hello volym toomodify den här inställningen.
      > 
      > 
      
        ![Lägg till volym](./media/storsimple-create-volume/AddVolume2-include.png)
4. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-create-volume/HCS_CheckIcon-include.png). En volym skapas med hello angivna inställningar.

![Video tillgänglig](./media/storsimple-create-volume/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur toocreate en StorSimple-volym klickar du på [här](https://azure.microsoft.com/documentation/videos/create-a-storsimple-volume/).

