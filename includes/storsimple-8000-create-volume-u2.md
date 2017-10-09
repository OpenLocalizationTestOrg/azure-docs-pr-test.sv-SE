<!--author=alkohli last changed: 07/19/2017-->

#### <a name="toocreate-a-volume"></a>toocreate en volym
1. Från hello tabular lista över hello enheter i hello **enheter** bladet Välj din enhet. Klicka på **+ Lägg till volymen**.

    ![Lägg till en ny volym](./media/storsimple-8000-create-volume-u2/step5createvol1.png)

2. I hello **lägga till en volym** bladet:
   
   1. Hej **Välj enhet** fältet fylls i automatiskt med din aktuella enhet.

   2. Hello listrutan, Välj hello volymbehållare där du behöver tooadd en volym. 

   3.  Anger du ett **namn** för volymen. Du kan byta namn på en volym när hello volymen har skapats.

   4. Välj hello på hello nedrullningsbara listan **typen** för volymen. För arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, väljer du en **lokalt fäst** volym. För all övrig data, väljer du en **nivåindelad** volym. Om du använder volymen för arkiveringsdata, markerar du **Använd volymen för arkiveringsdata med låg åtkomstfrekvens**.
      
       En nivåindelad volym är tunt etablerad och kan skapas snabbt. Att välja **Använd volymen för mindre ofta använda arkiveringsdata** för nivåindelade volymen för arkiveringsdata ändringar hello deduplicering segmentstorleken för volymen too512 KB. Om det här fältet inte är markerad använder hello motsvarande nivåindelade volymen en segmentstorlek på 64 KB. En större segmentstorlek för deduplicering kan hello enheten tooexpedite hello överföringen av stora mängder arkiveringsdata toohello moln.
       
       En lokalt Fäst volym etableras tjockt, vilket och säkerställer att hello primära data på hello volymen förblir lokala toohello enhet och inte läcker över toohello moln.  Om du skapar en lokalt Fäst volym hello enhet söker efter tillgängligt utrymme på hello lokala nivåerna tooprovision hello mängden hello begärda storlek. hello-åtgärden för att skapa en lokalt Fäst volym kan innebära att läcka befintlig data från hello enhet toohello moln och hello tidsåtgång toocreate hello volymen kanske är långt. hello totala tiden beror på hello storleken på hello etableras volym och tillgänglig nätverksbandbredd hello data på enheten.

   5. Ange hello **Etableringskapacitet** för volymen. Anteckna hello-kapaciteten som finns tillgänglig baserat på hello volymtyp som valts. hello angivna volymstorleken inte får överstiga hello tillgängligt utrymme.
      
       Du kan etablera lokalt fästa volymer upp too8.5 TB, eller nivåindelade volymer upp too200 TB på 8100 hello-enhet. Du kan etablera lokalt fästa volymer upp too22.5 TB, eller nivåindelade volymer upp too500 TB på hello större 8600-enheten. Eftersom lokalt utrymme på enheten hello krävs toohost hello arbetsminne nivåindelade volymer påverkar skapandet av fästa volymer hello diskutrymme för att etablera nivåindelade volymer. Därför minskar utrymmet som är tillgängligt för att skapa nivåindelade volymer om du skapar en lokalt fixerad volym. Om en nivåindelad volym skapas reduceras på samma sätt hello tillgängligt utrymme för att skapa en lokalt Fäst volym.
      
       Om du etablerar en lokalt Fäst volym på 8.5 TB (största tillåtna storleken) på din 8100-enhet har du uttömt alla hello lokalt tillgängligt utrymme på hello enhet. Du kan inte skapa någon nivåindelad volym från den tidpunkten och framåt eftersom det inte finns något lokalt utrymme ledigt på hello enheten toohost hello arbetsminne hello nivåer volym. Befintliga nivåindelade volymer påverkar också hello tillgängligt utrymme. Om du exempelvis har en 8100-enhet som redan har nivåindelade volymer på 106 TB finns det bara 4 TB utrymme tillgängligt för lokalt fästa volymer.

    6. I hello **anslutna värdar** klickar hello pilen. 

        ![Anslutna värdar](./media/storsimple-8000-create-volume-u2/step5createvol2.png)

    7. I hello **anslutna värdar** bladet Välj en befintlig ACR eller lägga till en ny ACR genom att utföra följande steg hello:

       1. Ange ett **namn** för din ACR.
       2. Under **iSCSI-Initierarnamn**, ange hello iSCSI kvalificerade namn (IQN) för Windows-värd. Om du inte har hello IQN, går för[Get hello IQN för en Windows Server-värd](#get-the-iqn-of-a-windows-server-host).

    9. Klicka på **Skapa**. En volym skapas med hello angivna inställningar.

        ![Klicka på Skapa](./media/storsimple-8000-create-volume-u2/step5createvol3.png)

        > [!NOTE]
        > Tänk på att hello volymen som du har skapat här inte skyddas. Du behöver toocreate och säkerhetskopieringsprinciper som associeras med den här volymen tootake schemalagda säkerhetskopieringar. 

