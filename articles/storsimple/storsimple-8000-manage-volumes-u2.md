---
title: aaaManage StorSimple-volymer (uppdatering 3) | Microsoft Docs
description: "Förklarar hur tooadd, ändra, övervaka och ta bort StorSimple-volymer och hur tootake dem offline om det behövs."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 6228c4486dd5a7887df670c4c4584c4edcdfc509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-volumes-update-3-or-later"></a>Använda hello StorSimple Enhetshanteraren service toomanage volymer (uppdatering 3 eller senare)

## <a name="overview"></a>Översikt

Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service toocreate och hantera volymer på hello StorSimple 8000-serien enheter som kör uppdatering 3 och senare.

Hej StorSimple enheten Manager-tjänsten är ett tillägg i hello Azure-portalen där du kan hantera din StorSimple-lösning från ett enda webbgränssnitt. Använd hello Azure portal toomanage volymer på alla dina enheter. Du kan också skapa och hantera StorSimple-tjänster, hantera enheter, principer för säkerhetskopiering och säkerhetskopieringskatalogen och visa aviseringar.

## <a name="volume-types"></a>Volymtyper

StorSimple-volymer kan vara:

* **Lokalt fästa volymer**: Data på dessa volymer finns kvar i hello lokala StorSimple-enhet när som helst.
* **Nivåindelade volymer**: Data i dessa volymer kan läcker över toohello moln.

En arkivering volym är en typ av nivåindelad volym. hello större segmentstorleken för deduplicering för arkivering volymer kan hello enheten tootransfer större segment av data toohello moln.

Om det behövs kan ändra du hello volymtyp från lokala tootiered eller nivåindelade toolocal. Mer information finns för[ändra hello volymtyp](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokalt fästa volymer

Lokalt fästa volymer är helt allokerade volymer som inte tjänstnivån data toohello molnet, garantier och säkerställer härigenom lokala för primära data, oberoende av molnet anslutningen. Data på lokalt fästa volymer är inte deduplicerad och komprimerade. dock dedupliceras ögonblicksbilder av lokalt fästa volymer. 

Lokalt fästa volymer är helt etablerad; Du måste därför ha tillräckligt med utrymme på enheten när de skapas. Du kan etablera lokalt fästa volymer upp tooa maximal storlek på 8 TB på hello StorSimple 8100-enhet och 20 TB på hello 8600-enhet. StorSimple reserverar hello återstående lokalt utrymme på hello enhet för ögonblicksbilder, metadata och databearbetning. Du kan öka hello storleken på en lokalt Fäst volym toohello maximalt tillgängligt utrymme, men du kan inte minska hello storleken på en volym skapas en gång.

När du skapar en lokalt Fäst volym, minskas hello tillgängligt utrymme för att skapa nivåindelade volymer. hello omvänd gäller även: Om du har befintliga nivåindelade volymer hello diskutrymme för att skapa lokalt fästa volymer blir lägre än hello gränser som anges ovan. Mer information om lokala volymer finns toohello [vanliga frågor och svar på lokalt fästa volymer](storsimple-8000-local-volume-faq.md).

### <a name="tiered-volumes"></a>Nivåindelade volymer

Nivåindelade volymer är tunt allokerade volymer i vilka hello ofta använda data är lokalt på hello enhet och mindre ofta använda data är automatiskt nivåer toohello moln. Tunn allokering är en virtualiseringsteknik som tillgängligt lagringsutrymme visas tooexceed fysiska resurser. I stället för att reservera tillräckligt med lagringsutrymme i förväg använder StorSimple tunn etablering tooallocate bara tillräckligt med utrymme toomeet aktuella kraven. hello elastisk uppbyggnad molnlagring underlättar den här metoden eftersom StorSimple kan öka eller minska molnet toomeet ändra lagringsbehov.

Om du använder hello nivåindelade volymen för arkiveringsdata, markerar hello **Använd volymen för mindre ofta använda arkiveringsdata** segmentstorleken för kryssrutan toochange hello deduplicering för volymen too512 KB. Om du inte väljer det här alternativet använder hello motsvarande nivåindelade volymen en segmentstorlek på 64 KB. En större segmentstorlek för deduplicering kan hello enheten tooexpedite hello överföringen av stora mängder arkiveringsdata toohello moln.


### <a name="provisioned-capacity"></a>Etablerad kapacitet

Läs toohello i den följande tabellen för maximal etablerad kapacitet för varje typ av enhet och volymen. (Observera att lokalt fästa volymer inte är tillgängliga på en virtuell enhet.)

|  | Maximal nivåindelad volymstorlek | Maximalt lokalt Fäst volymens storlek |
| --- | --- | --- |
| **Fysiska enheter** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Virtuella enheter** | | |
| 8010 |30 TB |Saknas |
| 8020 |64 TB |Saknas |

## <a name="hello-volumes-blade"></a>hello volymer bladet

Hej **volymer** bladet kan du toomanage hello lagringsvolymer som etablerats på hello Microsoft Azure StorSimple-enheten för dina initierarna (servrarna). Den visar hello lista över volymer i hello StorSimple-enheter anslutna tooyour-tjänsten.

 ![Volymsidan](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

En volym som består av en serie med attribut:

* **Volymnamn** – ett beskrivande namn som måste vara unikt och hjälper till att identifiera hello volym. Det här namnet används också övervaka rapporter när du filtrerar på en viss volym. När du har skapat hello volymen går inte att byta namn.
* **Status för** – kan vara online eller offline. Om en volym är offline, men det är inte synliga tooinitiators (servrar) som tillåts åtkomst toouse hello volym.
* **Kapacitet** – anger hello totala mängden data som kan lagras av hello initierare (server). Lokalt fästa volymer är helt etablerad och finns på hello StorSimple-enhet. Nivåindelade volymer är tunt etablerad och hello data är deduplicerad. Med tunt allokerade volymer allokera inte enheten före fysiska lagringskapacitet internt eller i hello moln enligt tooconfigured volymens kapacitet. hello volymens kapacitet är allokerade och används på begäran.
* **Typen** – anger om hello volymen är **skiktindelade** (hello standard) eller **lokalt Fäst**.

Använd hello instruktioner i den här självstudiekursen tooperform hello följande uppgifter:

* Lägg till en volym 
* Ändra en volym 
* Ändra hello volymtyp
* Ta bort en volym 
* Koppla från en volym 
* Övervaka en volym 

## <a name="add-a-volume"></a>Lägg till en volym

Du [har skapat en volym](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) under distributionen av enheten StorSimple 8000-serien. Lägger till en volym är en liknande procedur.

#### <a name="tooadd-a-volume"></a>tooadd en volym

1. Från hello tabular lista över hello enheter i hello **enheter** bladet Välj din enhet. Klicka på **+ Lägg till volymen**.

    ![Lägg till en ny volym](./media/storsimple-8000-manage-volumes-u2/step5createvol1.png)

2. I hello **lägga till en volym** bladet:
   
    1. Hej **Välj enhet** fältet fylls i automatiskt med din aktuella enhet.

    2. Hello listrutan, Välj hello volymbehållare där du behöver tooadd en volym.

    3.  Anger du ett **namn** för volymen. När hello volymen har skapats kan du byta namn på hello volym.

    4. Välj hello på hello nedrullningsbara listan **typen** för volymen. För arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, väljer du en **lokalt fäst** volym. För all övrig data, väljer du en **nivåindelad** volym. Om du använder volymen för arkiveringsdata, markerar du **Använd volymen för arkiveringsdata med låg åtkomstfrekvens**.
      
       En nivåindelad volym är tunt etablerad och kan skapas snabbt. Att välja **Använd volymen för mindre ofta använda arkiveringsdata** för nivåindelade volymen för arkiveringsdata ändringar hello deduplicering segmentstorleken för volymen too512 KB. Om det här fältet inte är markerad använder hello motsvarande nivåindelade volymen en segmentstorlek på 64 KB. En större segmentstorlek för deduplicering kan hello enheten tooexpedite hello överföringen av stora mängder arkiveringsdata toohello moln.
       
       En lokalt Fäst volym etableras tjockt, vilket och säkerställer att hello primära data på hello volymen förblir lokala toohello enhet och inte läcker över toohello moln.  Om du skapar en lokalt Fäst volym hello enhet söker efter tillgängligt utrymme på hello lokala nivåerna tooprovision hello mängden hello begärda storlek. hello-åtgärden för att skapa en lokalt Fäst volym kan innebära att läcka befintlig data från hello enhet toohello moln och hello tidsåtgång toocreate hello volymen kanske är långt. hello totala tiden beror på hello storleken på hello etableras volym och tillgänglig nätverksbandbredd hello data på enheten.

    5. Ange hello **Etableringskapacitet** för volymen. Anteckna hello-kapaciteten som finns tillgänglig baserat på hello volymtyp som valts. hello angivna volymstorleken inte får överstiga hello tillgängligt utrymme.
      
       Du kan etablera lokalt fästa volymer upp too8.5 TB, eller nivåindelade volymer upp too200 TB på 8100 hello-enhet. Du kan etablera lokalt fästa volymer upp too22.5 TB, eller nivåindelade volymer upp too500 TB på hello större 8600-enheten. Eftersom lokalt utrymme på enheten hello krävs toohost hello arbetsminne nivåindelade volymer påverkar skapandet av fästa volymer hello diskutrymme för att etablera nivåindelade volymer. Därför minskar utrymmet som är tillgängligt för att skapa nivåindelade volymer om du skapar en lokalt fixerad volym. Om en nivåindelad volym skapas reduceras på samma sätt hello tillgängligt utrymme för att skapa en lokalt Fäst volym.
      
       Om du etablerar en lokalt Fäst volym på 8.5 TB (största tillåtna storleken) på din 8100-enhet har du uttömt alla hello lokalt tillgängligt utrymme på hello enhet. Du kan inte skapa någon nivåindelad volym från den tidpunkten och framåt eftersom det inte finns något lokalt utrymme ledigt på hello enheten toohost hello arbetsminne hello nivåer volym. Befintliga nivåindelade volymer påverkar också hello tillgängligt utrymme. Om du exempelvis har en 8100-enhet som redan har nivåindelade volymer på 106 TB finns det bara 4 TB utrymme tillgängligt för lokalt fästa volymer.

    6. I hello **anslutna värdar** klickar hello pilen. 

        ![Anslutna värdar](./media/storsimple-8000-manage-volumes-u2/step5createvol2.png)

    7. I hello **anslutna värdar** bladet Välj en befintlig ACR eller Lägg till en ny ACR. Om du väljer en ny ACR anger en **namn** för din ACR anger hello **iSCSI Qualified Name** (IQN) för Windows-värd. Om du inte har hello IQN, går för[Get hello IQN för en Windows Server-värd](#get-the-iqn-of-a-windows-server-host). Klicka på **Skapa**. En volym skapas med hello angivna inställningar.

        ![Klicka på Skapa](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Den nya volymen är nu redo toouse.

> [!NOTE]
> Om du skapar en lokalt Fäst volym och sedan skapa en annan lokalt Fäst volym omedelbart efteråt hello jobb för skapande av volymen körs sekventiellt. hello första volym för att skapa jobb måste slutföras innan hello nästa volym för att skapa jobb kan börja.

## <a name="modify-a-volume"></a>Ändra en volym

Ändra en volym när du behöver tooexpand den eller ändra hello värdar som har åtkomst till hello volym.

> [!IMPORTANT]
> * Om du ändrar hello volymens storlek på hello enhet måste hello volymens storlek toobe ändras på hello-värden.
> * hello värden på klientsidan stegen som beskrivs här är för Windows Server 2012 (2012R2). Procedurer för Linux eller andra värdoperativsystem är olika. Läs tooyour värden operativsystemet anvisningar när du ändrar hello volym på en värd som kör ett annat operativsystem.

#### <a name="toomodify-a-volume"></a>toomodify en volym

1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **enheter**. Hello tabular lista över hello enheter, Välj hello-enhet som har hello volym som du avser toomodify. Klicka på **Inställningar > volymer**.

    ![Gå tooVolumes bladet](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. Välj hello volym från hello tabular lista över volymer, och högerklicka på snabbmenyn för tooinvoke hello. Välj **ta offline** tootake hello volym som du vill ändra offline.

    ![Välj och kopplar från volymen](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. I hello **ta offline** bladet granska hello effekten av att hello volym tas offline och välj hello motsvarande kryssruta. Se till att hello motsvarande volymen på hello värden är offline först. Information om hur tootake en volym som är offline på värdservern anslutna tooStorSimple finns specifika anvisningar för toooperating system. Klicka på **ta offline**.

    ![Granska effekten av att volym tas offline](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. När hello volymen är offline (som visas av hello volymstatusen), Välj hello volym och högerklicka på snabbmenyn för tooinvoke hello. Välj **ändra volym**.

    ![Välj Ändra volym](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. I hello **ändra volym** bladet kan du hello följande ändringar:
   
   1. Hej volym **namn** kan inte redigeras.
   2. Konvertera hello **typen** från lokalt Fäst tootiered eller nivåindelade toolocally Fäst (se [ändra hello volymtyp](#change-the-volume-type) mer information).
   3. Öka hello **Etableringskapacitet**. Hej **Etableringskapacitet** kan bara ökas. Det går inte att komprimera en volym när den har skapats.
   4. Under **anslutna värdar**, kan du ändra hello ACR. toomodify en ACR hello volymen måste vara offline.

       ![Granska effekten av att volym tas offline](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Klicka på **spara** toosave ändringarna. Klicka på **Ja** när du uppmanas att bekräfta åtgärden. hello Azure-portalen visas ett meddelande om uppdatering volym. Ett meddelande visas när hello volymen har uppdaterats.

    ![Granska effekten av att volym tas offline](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Om du expanderar en volym, utför följande steg på värddatorn Windows hello:
   
   1. Gå för**Datorhantering** ->**Diskhantering**.
   2. Högerklicka på **Diskhantering** och välj **Skanna diskar**.
   3. Välj hello volym som du har uppdaterat, högerklicka och välj sedan i hello lista över diskar, **Utöka volym**. startar guiden för hello Utöka volym. Klicka på **Nästa**.
   4. Slutför guiden för hello, acceptera hello standardvärden. När hello guiden är klar ska hello volymen visa hello ökad storlek.
      
      > [!NOTE]
      > Om du expanderar en lokalt Fäst volym och sedan Fäst en annan lokalt volym omedelbart efteråt hello volym expansion jobb körs sekventiellt. hello första volym expansion jobb måste slutföras innan hello nästa volym expansion jobb kan börja.
      

## <a name="change-hello-volume-type"></a>Ändra hello volymtyp

Du kan ändra hello volymtyp från nivåindelade toolocally Fäst eller lokalt Fäst tootiered. Den här konverteringen får inte vara en frekventa förekomst.

### <a name="tiered-toolocal-volume-conversion-considerations"></a>Nivåindelad toolocal volym tänka på vid konvertering

Några skäl för att konvertera en volym från nivåindelade toolocally Fäst är:

* Lokala garantier avseende datatillgänglighet och prestanda
* Eliminering av molnet svarstiderna och molnet anslutningsproblem.

Dessa är oftast små befintliga volymer som du vill tooaccess ofta. En lokalt Fäst volym etableras fullständigt när den skapas. 

Om du konverterar en nivåindelad volym tooa lokalt Fäst volym, kontrollerar StorSimple att du har tillräckligt med utrymme på enheten innan den börjar hello konvertering. Om du har inte tillräckligt med utrymme du får ett felmeddelande och hello åtgärden avbryts. 

> [!NOTE]
> Innan du börjar konvertering från nivåindelade toolocally Fäst, kontrollera att du överväger hello kraven på diskutrymme på andra arbetsbelastningar. 

Konverteringen från en skiktad tooa lokalt Fäst volym kan Enhetsprestanda försämras. Hello följande faktorer kan dessutom öka hello tid det tar toocomplete hello konvertering:

* Det finns inte tillräckligt bandbredd.
* Det finns ingen aktuell säkerhetskopia.

toominimize hello effekter som kan ha följande faktorer:

* Granska dina bandbreddsbegränsning principer och se till att det finns en dedikerad 40 Mbit/s bandbredd.
* Schemalägga hello konvertering för låg belastning.
* Ta en ögonblicksbild i molnet innan du börjar hello konvertering.

Om du konverterar flera volymer (stöd för olika arbetsbelastningar) bör du prioritera hello Volymkonvertering så att den högre prioritet volymer konverteras först. Du bör till exempel konvertera volymer som är värdar för virtuella datorer (VM) eller volymer med SQL arbetsbelastningar innan du konverterar volymer med filen resursen arbetsbelastningar.

### <a name="local-tootiered-volume-conversion-considerations"></a>Lokala tootiered volym tänka på vid konvertering

Du kanske vill toochange en lokalt Fäst volym tooa nivåindelade volymen om du behöver ytterligare utrymme tooprovision andra volymer. När du konverterar hello lokalt Fäst volym tootiered ökar hello tillgänglig kapacitet på hello enheten med hello storleken på hello publicerat kapacitet. Om problem med nätverksanslutningen förhindra hello konverteringen av en volym från hello lokala typen toohello nivåer typ, hello lokal volym kommer att ha egenskaperna för en nivåindelad volym tills hello konverteringen har slutförts. Det beror på att vissa data kan ha hamnat toohello moln. Den här spilled data fortsätter toooccupy lokalt utrymme på hello-enhet som inte går att frigöra tills hello åtgärden startas om och slutförs.

> [!NOTE]
> Konvertera en volym kan ta lite tid och du kan inte avbryta en konvertering när den har startat. hello volymen är online under hello konverteringen, och du kan göra säkerhetskopior, men du kan inte expandera eller återställa hello volym medan hello konverteringen sker.


#### <a name="toochange-hello-volume-type"></a>toochange hello volymtyp

1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **enheter**. Hello tabular lista över hello enheter, Välj hello-enhet som har hello volym som du avser toomodify. Klicka på **Inställningar > volymer**.

    ![Gå tooVolumes bladet](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Välj hello volym från hello tabular lista över volymer, och högerklicka på snabbmenyn för tooinvoke hello. Välj **ändra**.

    ![Välj Ändra snabbmenyn](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. På hello **ändra volym** bladet ändra hello volymtyp genom att välja hello nya typen hello **typen** listrutan.
   
   * Om du ändrar hello typ för**lokalt Fäst**, StorSimple kontrollerar toosee om det finns tillräckligt med kapacitet.
   * Om du ändrar hello typ för**skiktindelade** och kommer att användas volymen för arkiveringsdata, markerar hello **Använd volymen för mindre ofta använda arkiveringsdata** kryssrutan.
   * Om du konfigurerar en lokalt Fäst volym som nivåer eller _vice versa_, hello visas följande meddelande.
   
    ![Ändra volym typen meddelande](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Klicka på **spara** toosave hello ändringar. När du uppmanas att bekräfta, klickar du på **Ja** toostart hello konverteringen. 

    ![Spara och bekräfta](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. hello Azure-portalen visar ett meddelande för att skapa en hello jobb som skulle uppdateras hello volym. Klicka på hello meddelande toomonitor hello hello volym konvertering jobbets status.

    ![Jobb för Volymkonvertering](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>Koppla från en volym

Du kan behöva tootake en volym offline när du planerar toomodify eller ta bort hello volym. När en volym är offline, är den inte tillgänglig för skrivskyddad åtkomst. Du måste koppla hello volymen offline på hello värden och hello enheten.

#### <a name="tootake-a-volume-offline"></a>tootake en volym som är offline

1. Kontrollera att hello volymen i fråga inte används innan den tas offline.
2. Ta hello volymen offline på värden för hello första. Detta tar bort alla potentiella risken för korrupta data på hello volym. Specifika anvisningar finns i toohello instruktioner för operativsystemet för värden.
3. Efter hello värden är offline, ta hello volymen på hello enhet offline genom att utföra hello följande steg:
   
    1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **enheter**. Hello tabular lista över hello enheter, Välj hello-enhet som har hello volym som du avser toomodify. Klicka på **Inställningar > volymer**.

        ![Gå tooVolumes bladet](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. Välj hello volym från hello tabular lista över volymer, och högerklicka på snabbmenyn för tooinvoke hello. Välj **ta offline** tootake hello volym som du vill ändra offline.

        ![Välj och kopplar från volymen](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. I hello **ta offline** bladet granska hello effekten av att hello volym tas offline och välj hello motsvarande kryssruta. Klicka på **ta offline**. 

    ![Granska effekten av att volym tas offline](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      Du meddelas när hello volym är offline. hello volymstatusen uppdateras även tooOffline.
      
4. När en volym är offline, om du väljer hello volym och högerklicka, **Anslut** alternativet blir tillgängligt i hello snabbmenyn.

> [!NOTE]
> Hej **ta Offline** kommando skickar en begäran toohello enhet tootake hello volym offline. Om värdar fortfarande använder hello volymen, detta resulterar i avbrutna anslutningar, men inte koppla hello volymen misslyckas.

## <a name="delete-a-volume"></a>Ta bort en volym

> [!IMPORTANT]
> Du kan ta bort en volym endast om den är offline.

Slutför följande steg toodelete en volym hello.

#### <a name="toodelete-a-volume"></a>toodelete en volym

1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **enheter**. Hello tabular lista över hello enheter, Välj hello-enhet som har hello volym som du avser toomodify. Klicka på **Inställningar > volymer**.

    ![Gå tooVolumes bladet](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Kontrollera status för hello hello volym som du vill toodelete. Om hello volym toodelete inte är offline, koppla från den först. Gör så hello i [kopplar från en volym](#take-a-volume-offline).
4. När hello volymen är offline, välja hello volym, högerklickar du på tooinvoke hello snabbmenyn och välj sedan **ta bort**.

    ![Välj Ta bort från snabbmenyn](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. I hello **ta bort** bladet, granska och välj hello kryssrutan mot hello effekten av att ta bort en volym. När du tar bort en volym, förloras alla hello-data som finns på hello volym. 

    ![Spara och bekräfta ändringar](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. När hello volymen tas bort, uppdaterar hello tabular lista över volymer tooindicate hello borttagning.

    ![Uppdaterade Volymlista](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Om du tar bort en lokalt Fäst volym uppdateras hello diskutrymme för nya volymer inte omedelbart. Hej StorSimple Enhetshanteraren Service uppdaterar regelbundet hello lokalt tillgängligt utrymme. Vi rekommenderar att du vänta några minuter innan du försöker toocreate hello ny volym.
   >
   > Om du tar bort en lokalt Fäst volym och ta bort en annan lokalt Fäst dessutom volymen omedelbart efteråt hello volym jobb körs sekventiellt. hello jobbet första volym måste slutföras innan hello jobbet nästa volym kan börja.

## <a name="monitor-a-volume"></a>Övervaka en volym

Övervakning av volym kan du toocollect I/O-relaterade statistik för en volym. Övervakning är aktiverad som standard för hello första 32 volymer som du skapar. Övervakning av ytterligare volymer är inaktiverat som standard. 

> [!NOTE]
> Övervakning av klonade volymer är inaktiverat som standard.


Utför följande steg tooenable hello eller inaktivera övervakning för en volym.

#### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable eller inaktivera övervakning av volym

1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **enheter**. Hello tabular lista över hello enheter, Välj hello-enhet som har hello volym som du avser toomodify. Klicka på **Inställningar > volymer**.
2. Välj hello volym från hello tabular lista över volymer, och högerklicka på snabbmenyn för tooinvoke hello. Välj **ändra**.
3. I hello **ändra volym** bladet för **övervakning** Välj **aktivera** eller **inaktivera** tooenable eller inaktivera övervakning.

    ![Inaktivera övervakning](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. Klicka på **spara** och när du uppmanas att bekräfta på **Ja**. hello Azure-portalen visar ett meddelande för att uppdatera hello volym och ett meddelande när hello volymen har uppdaterats.

## <a name="next-steps"></a>Nästa steg

* Lär dig hur för[klona en StorSimple-volym](storsimple-8000-clone-volume-u2.md).
* Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

