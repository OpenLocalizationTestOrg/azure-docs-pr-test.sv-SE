<!--author=alkohli last changed: 06/22/17-->

#### <a name="toocreate-a-volume-container"></a>toocreate en volymbehållare
1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**. Välj hello tabular lista över hello enheter, och klicka på en enhet. 

    ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer1.png)

2. I hello enheten instrumentpanelen, klickar du på **+ Lägg till volymbehållare**

    ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

3. I hello **Lägg till volymbehållare** bladet:
   
   1. hello enhet väljs automatiskt.
   2. Ange ett **namn** för din volymbehållare. hello namn måste vara 3 too32 tecken. Du kan byta namn på en volymbehållare när den har skapats.
   3. Välj **aktivera molnet Lagringskryptering** tooenable kryptering av hello data som skickas från hello enhet toohello moln.
   4. Ange och bekräfta en **krypteringsnyckel för Molnlagring** som är 8 too32 tecken. Den här nyckeln används av hello tooaccess krypterade data på enheten.
   5. Välj en **Lagringskonto** tooassociate med volymbehållaren. Du kan välja ett befintligt lagringskonto eller hello standardkonto som genereras när hello dags att skapa en tjänst. Du kan också använda hello **Lägg till ny** alternativet toospecify ett lagringskonto som inte är länkad toothis prenumeration på tjänsten.
   6. Välj **obegränsad** i hello **ange bandbredd** nedrullningsbara listan om du inte vill tooconsume all hello tillgänglig bandbredd. Du kan också ange det här alternativet för**anpassad** tooemploy bandbreddskontroller, och ange ett värde mellan 1 och 1 000 Mbps.
      Om du har din bandbredd användningsinformation som är tillgängliga kan du kanske kan tooallocate bandbredd enligt ett schema genom att ange **Välj en bandbreddsmall**. Stegvisa anvisningar finns för[Lägg till en bandbreddsmall](../articles/storsimple/storsimple-8000-manage-bandwidth-templates.md#add-a-bandwidth-template).

      ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer6b.png)
   7. Klicka på **Skapa**.

        ![Blad för volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer6.png)
   
       Du meddelas när hello volymbehållare har skapats.

       ![Meddelande om att volymbehållaren har skapats](./media/storsimple-8000-create-volume-container/createvolumecontainer8.png)

   hello nyskapad volymbehållare listas i hello lista över volymbehållare för din enhet.

   ![Bladet Lägg till volymbehållare](./media/storsimple-8000-create-volume-container/createvolumecontainer9.png)


