<!--author=alkohli last changed: 08/16/2016-->

#### <a name="toocreate-a-volume"></a>toocreate en volym
1. På hello enhet **Snabbstart** klickar du på **lägga till en volym** toostart hello guiden Lägg till en volym.
2. I hello guiden Lägg till en volym, under **grundläggande inställningar**:
   
   1. Anger du ett **namn** för volymen.
   2. Välj hello på hello nedrullningsbara listan **användningstyp** för volymen. För arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, väljer du en **lokalt fäst** volym. För all övrig data, väljer du en **nivåindelad** volym. Om du använder volymen för arkiveringsdata, markerar du **Använd volymen för arkiveringsdata med låg åtkomstfrekvens**. 
      
       En lokalt Fäst volym etableras tjockt, vilket och säkerställer att hello primära data på hello volymen förblir lokala toohello enhet och inte läcker över toohello moln.  Om du skapar en lokalt Fäst volym hello enhet söker efter tillgängligt utrymme på hello lokala nivåerna tooprovision hello mängden hello begärda storlek. hello-åtgärden för att skapa en lokalt Fäst volym kan innebära att läcka befintlig data från hello enhet toohello moln och hello tidsåtgång toocreate hello volymen kanske är långt. hello totala tiden beror på hello storleken på hello etableras volym och tillgänglig nätverksbandbredd hello data på enheten. 
      
       En nivåindelad volym är tunt etablerad och kan skapas snabbt. Att välja **Använd volymen för mindre ofta använda arkiveringsdata** för nivåindelade volymen för arkiveringsdata ändringar hello deduplicering segmentstorleken för volymen too512 KB. Om det här fältet inte är markerad använder hello motsvarande nivåindelade volymen en segmentstorlek på 64 KB. En större segmentstorlek för deduplicering kan hello enheten tooexpedite hello överföringen av stora mängder arkiveringsdata toohello moln.
   3. Ange hello **Etableringskapacitet** för volymen. Anteckna hello-kapaciteten som finns tillgänglig baserat på hello volymtyp som valts. hello angivna volymstorleken inte får överstiga hello tillgängligt utrymme.
      
       Du kan etablera lokalt fästa volymer upp too8.5 TB, eller nivåindelade volymer upp too200 TB på 8100 hello-enhet. Du kan etablera lokalt fästa volymer upp too22.5 TB, eller nivåindelade volymer upp too500 TB på hello större 8600-enheten. Eftersom lokalt utrymme på enheten hello krävs toohost hello arbetsminne nivåindelade volymer påverkar skapandet av fästa volymer hello diskutrymme för att etablera nivåindelade volymer. Om du skapar en lokalt fäst volym, kommer därmed utrymmet som finns tillgängligt för att skapa nivåindelade volymer att minska. Om en nivåindelad volym skapas reduceras på samma sätt hello tillgängligt utrymme för att skapa en lokalt Fäst volym.
      
       Om du etablerar en lokalt Fäst volym på 8.5 TB (största tillåtna storleken) på din 8100-enhet har du uttömt alla hello lokalt tillgängligt utrymme på hello enhet. Du kommer inte att kunna toocreate någon nivåindelad volym från att och senare som det är något lokalt utrymme ledigt på hello enheten toohost hello arbetsminne hello nivåer volym. Befintliga nivåindelade volymer påverkar också hello tillgängligt utrymme. Om du exempelvis har en 8100-enhet som redan har nivåindelade volymer på 106 TB finns det bara 4 TB utrymme tillgängligt för lokalt fästa volymer.
      
       hello följande bild visar hello **grundläggande inställningar** dialogrutan för en lokalt Fäst volym.
      
        ![Lägg till lokal volym](./media/storsimple-create-volume-u2/add-local-volume-include.png)
      
       hello följande bild visar hello **grundläggande inställningar** dialogrutan för en nivåindelad volym.
      
        ![Lägg till lokal volym](./media/storsimple-create-volume-u2/add-tiered-volume-include.png)
   
   1. Klicka på pilikonen hello ![pilikon](./media/storsimple-create-volume-u2/HCS_ArrowIcon-include.png) toogo toohello nästa sida.
3. I hello **ytterligare inställningar** dialogrutan Lägg till en ny åtkomstkontrollpost (ACR):
   
   1. Ange ett **namn** för din ACR.
   2. Under **iSCSI-Initierarnamn**, ange hello iSCSI kvalificerade namn (IQN) för Windows-värd. Om du inte har hello IQN, går för[Get hello IQN för en Windows Server-värd](#get-the-iqn-of-a-windows-server-host).
   3. Under **standard säkerhetskopiering för den här volymen?**väljer hello **aktivera** kryssrutan. hello standard säkerhetskopieringen skapar en princip som körs klockan 22.30 varje dag (enhetens tid) och skapar en ögonblicksbild av den här volymen i molnet.
      
      > [!NOTE]
      > När hello säkerhetskopieringen har aktiverats här, kan den inte återställas. Tooedit hello volym toomodify måste den här inställningen.
      > 
      > 
      
      ![Lägg till volym](./media/storsimple-create-volume-u2/AddVolumeAdditionalSettings1.png)
4. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-create-volume-u2/HCS_CheckIcon-include.png). En volym skapas med hello angivna inställningar.

