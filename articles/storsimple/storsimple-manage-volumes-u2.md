---
title: aaaManage StorSimple-volymer (U2) | Microsoft Docs
description: "Förklarar hur tooadd, ändra, övervaka och ta bort StorSimple-volymer och hur tootake dem offline om det behövs."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 57896932-0aa5-4805-970c-d13403ae7551
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/28/2016
ms.author: alkohli
ms.openlocfilehash: 8b64f1d92023646cdf881882854d9bc5d7334a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-volumes-update-2"></a>Använda hello StorSimple Manager-tjänsten toomanage volymer (uppdatering 2)
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur toouse hello toocreate för StorSimple Manager-tjänsten och hantera volymer på hello StorSimple-enhet och virtuella StorSimple-enheten med uppdatering 2.

Hej StorSimple Manager-tjänsten är ett tillägg i hello klassiska Azure-portalen där du kan hantera din StorSimple-lösning från ett enda webbgränssnitt. I tillägg toomanaging volymer, kan du använda hello StorSimple Manager-tjänsten toocreate och hantera StorSimple-tjänster, visa och hantera enheter, Visa aviseringar, och visa och hantera principer för säkerhetskopiering och hello säkerhetskopieringskatalogen.

## <a name="volume-types"></a>Volymtyper
StorSimple-volymer kan vara:

* **Lokalt fästa volymer**: Data på dessa volymer finns kvar i hello lokala StorSimple-enhet när som helst.
* **Nivåindelade volymer**: Data i dessa volymer kan läcker över toohello moln.

En arkivering volym är en typ av nivåindelad volym. hello större segmentstorleken för deduplicering för arkivering volymer kan hello enheten tootransfer större segment av data toohello moln. 

Om det behövs kan ändra du hello volymtyp från lokala tootiered eller nivåindelade toolocal. Mer information finns för[ändra hello volymtyp](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokalt fästa volymer
Lokalt fästa volymer är helt allokerade volymer som inte tjänstnivån data toohello molnet, garantier och säkerställer härigenom lokala för primära data, oberoende av molnet anslutningen. Data på lokalt fästa volymer är inte deduplicerad och komprimerade. dock dedupliceras ögonblicksbilder av lokalt fästa volymer. 

Lokalt fästa volymer är helt etablerad; Du måste därför ha tillräckligt med utrymme på enheten när de skapas. Du kan etablera lokalt fästa volymer upp tooa maximal storlek på 8 TB på hello StorSimple 8100-enhet och 20 TB på hello 8600-enhet. StorSimple reserverar hello återstående lokalt utrymme på hello enhet för ögonblicksbilder, metadata och databearbetning. Du kan öka hello storleken på en lokalt Fäst volym toohello maximalt tillgängligt utrymme, men du kan inte minska hello storleken på en volym skapas en gång.

När du skapar en lokalt Fäst volym, minskas hello tillgängligt utrymme för att skapa nivåindelade volymer. hello omvänd gäller även: Om du har befintliga nivåindelade volymer hello diskutrymme för att skapa lokalt fästa volymer blir lägre än hello gränser som anges ovan. Mer information om lokala volymer finns toohello [vanliga frågor och svar på lokalt fästa volymer](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Nivåindelade volymer
Nivåindelade volymer är tunt allokerade volymer i vilka hello ofta använda data är lokalt på hello enhet och mindre ofta använda data är automatiskt nivåer toohello moln. Tunn allokering är en virtualiseringsteknik som tillgängligt lagringsutrymme visas tooexceed fysiska resurser. I stället för att reservera tillräckligt med lagringsutrymme i förväg använder StorSimple tunn etablering tooallocate bara tillräckligt med utrymme toomeet aktuella kraven. hello elastisk uppbyggnad molnlagring underlättar den här metoden eftersom StorSimple kan öka eller minska molnet toomeet ändra lagringsbehov.

Om du använder hello nivåindelade volymen för arkiveringsdata, markerar hello **Använd volymen för mindre ofta använda arkiveringsdata** kryssrutan ändrar hello segmentstorleken för deduplicering för volymen too512 KB. Om du inte väljer det här alternativet använder hello motsvarande nivåindelade volymen en segmentstorlek på 64 KB. En större segmentstorlek för deduplicering kan hello enheten tooexpedite hello överföringen av stora mängder arkiveringsdata toohello moln.

> [!NOTE]
> Arkivering volymer som har skapats med en före uppdateringen 2 version av StorSimple kommer att importeras som har delats med hello arkivering kryssrutan är markerad.
> 
> 

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

## <a name="hello-volumes-page"></a>hello volymer sida
Hej **volymer** sidan kan du toomanage hello lagringsvolymer som etablerats på hello Microsoft Azure StorSimple-enheten för dina initierarna (servrarna). Den visar hello lista över volymer på StorSimple-enheten.

 ![Volymsidan](./media/storsimple-manage-volumes-u2/VolumePage.png)

En volym som består av en serie med attribut:

* **Volymnamn** – ett beskrivande namn som måste vara unikt och hjälper till att identifiera hello volym. Det här namnet används också övervaka rapporter när du filtrerar på en viss volym.
* **Status för** – kan vara online eller offline. Om en volym är offline, men det är inte synliga tooinitiators (servrar) som tillåts åtkomst toouse hello volym.
* **Kapacitet** – anger hello totala mängden data som kan lagras av hello initierare (server). Lokalt fästa volymer är helt etablerad och finns på hello StorSimple-enhet. Nivåindelade volymer är tunt etablerad och hello data är deduplicerad. Med tunt allokerade volymer allokera inte enheten före fysiska lagringskapacitet internt eller i hello moln enligt tooconfigured volymens kapacitet. hello volymens kapacitet är allokerade och används på begäran.
* **Typen** – anger om hello volymen är **skiktindelade** (hello standard) eller **lokalt Fäst**.
* **Säkerhetskopiering** – anger om det finns en standardprincip för säkerhetskopiering för hello volym.
* **Åtkomst** – anger hello initierarna (servrarna) som tillåts åtkomst toothis volym. Initierare som inte är medlemmar i åtkomstkontrollpost (ACR) som är associerad med hello volymen kan inte se hello volym.
* **Övervaka** – anger huruvida en volym som övervakas. En volym har övervakning aktiverad som standard när den skapas. Övervakning kommer dock, inaktiveras för en volym kloning. tooenable övervakning för en volym, följer du anvisningarna hello i [övervaka en volym](#monitor-a-volume). 

Använd hello instruktioner i den här självstudiekursen tooperform hello följande uppgifter:

* Lägg till en volym 
* Ändra en volym 
* Ändra hello volymtyp
* Ta bort en volym 
* Koppla från en volym 
* Övervaka en volym 

## <a name="add-a-volume"></a>Lägg till en volym
Du [har skapat en volym](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) under distributionen av din StorSimple-lösning. Lägger till en volym är en liknande procedur.

#### <a name="tooadd-a-volume"></a>tooadd en volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj en volymbehållare hello listan och dubbelklicka på den tooaccess hello volymer som är associerade med hello-behållare.
3. Klicka på **Lägg till** på hello hello sidans nederkant. hello guiden Lägg till en volym startar.
   
     ![Lägg till volym slutförs grundläggande inställningar](./media/storsimple-manage-volumes-u2/TieredVolEx.png)
4. I hello guiden Lägg till en volym, under **grundläggande inställningar**, hello följande:
   
   1. Ange ett **namn** för volymen.
   2. Välj en **användningstyp** hello nedrullningsbara listan. Arbetsbelastningar som kräver data toobe tillgänglig lokalt på hello enhet vid alla tidpunkter, Välj **lokalt Fäst**. Alla andra typer av data, Välj **skiktindelade**. (**Skiktindelade** hello standard.)
   3. Om du har valt **skiktindelade** i steg 2, kan du välja hello **Använd volymen för mindre ofta använda arkiveringsdata** kryssrutan tooconfigure en Arkiverings-volym.
   4. Ange hello **Etableringskapacitet** för volymen i GB eller TB. Se [etablerad kapacitet](#provisioned-capacity) för maximal storlek för varje typ av enhet och volymen. Titta på hello **tillgänglig kapacitet** toodetermine hur mycket lagringsutrymme som är tillgängligt på enheten.
5. Klicka på pilikonen hello![Pilikonen](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Om du konfigurerar en lokalt Fäst volym, visas följande meddelande hello.
   
    ![Ändra volym typen meddelande](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
6. Klicka på pilikonen hello ![pilikonen](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)igen toogo toohello **ytterligare inställningar** sidan.
   
    ![Lägg till ytterligare inställningar för volym-guiden](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>
7. Under **ytterligare inställningar**, lägga till en ny åtkomstkontrollpost (ACR):
   
   1. Välj en åtkomstkontrollpost (ACR) hello nedrullningsbara listan. Alternativt kan du lägga till en ny ACR. ACRs avgör vilka värdar som kan komma åt dina volymer av matchande hello värd IQN med som hello posten i listan. Om du inte anger en ACR visas hello efter meddelande.
      
        ![Ange ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)
   2. Vi rekommenderar att du väljer hello **aktiverar en standard säkerhetskopiering för den här volymen** kryssrutan.
   3. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) toocreate hello volym med hello angivna inställningar.

Den nya volymen är nu redo toouse.

> [!NOTE]
> Om du skapar en lokalt Fäst volym och sedan skapa en annan lokalt Fäst volym omedelbart efteråt hello jobb för skapande av volymen körs sekventiellt. hello första volym för att skapa jobb måste slutföras innan hello nästa volym för att skapa jobb kan börja.
> 
> 

## <a name="modify-a-volume"></a>Ändra en volym
Ändra en volym när du behöver tooexpand den eller ändra hello värdar som har åtkomst till hello volym.

> [!IMPORTANT]
> * Om du ändrar hello volymens storlek på hello enhet måste hello volymens storlek toobe ändras på hello-värden. 
> * hello värden på klientsidan stegen som beskrivs här är för Windows Server 2012 (2012R2). Procedurer för Linux eller andra värdoperativsystem är olika. Läs tooyour värden operativsystemet anvisningar när du ändrar hello volym på en värd som kör ett annat operativsystem. 
> 
> 

#### <a name="toomodify-a-volume"></a>toomodify en volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj en volymbehållare hello listan och dubbelklicka på den tooview hello volymer som är associerade med hello-behållare.
3. Välj en volym och hello längst hello-sidan, klickar du på **ändra**. hello startas ändra volym.
4. Hello ändra i guiden för volym under **grundläggande inställningar**, kan du göra hello följande:
   
   * Redigera hello **namn**.
   * Konvertera hello **användningstyp** från lokalt Fäst tootiered eller nivåindelade toolocally Fäst (se [ändra hello volymtyp](#change-the-volume-type) mer information).
   * Öka hello **Etableringskapacitet**. Hej **Etableringskapacitet** kan bara ökas. Det går inte att komprimera en volym när den har skapats.
5. Under **ytterligare inställningar**, kan du ändra hello ACR, förutsatt att hello volymen är offline. Om hello volymen är online, behöver du tootake det offline först. Se toohello stegen i [kopplar från en volym](#take-a-volume-offline) tidigare toomodifying hello ACR.
   
   > [!NOTE]
   > Du kan inte ändra hello **aktiverar en standard säkerhetskopiering** alternativet för hello volym.
   > 
   > 
6. Spara dina ändringar genom att klicka på kryssikonen hello ![kryssikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). hello klassiska Azure-portalen visas ett meddelande om uppdatering volym. Ett meddelande visas när hello volymen har uppdaterats.
7. Om du expanderar en volym, utför följande steg på värddatorn Windows hello:
   
   1. Gå för**Datorhantering** ->**Diskhantering**.
   2. Högerklicka på **Diskhantering** och välj **Skanna diskar**.
   3. Välj hello volym som du har uppdaterat, högerklicka och välj sedan i hello lista över diskar, **Utöka volym**. startar guiden för hello Utöka volym. Klicka på **Nästa**.
   4. Slutför guiden för hello, acceptera hello standardvärden. När hello guiden är klar ska hello volymen visa hello ökad storlek.
      
      > [!NOTE]
      > Om du expanderar en lokalt Fäst volym och sedan Fäst en annan lokalt volym omedelbart efteråt hello volym expansion jobb körs sekventiellt. hello första volym expansion jobb måste slutföras innan hello nästa volym expansion jobb kan börja.
      > 
      > 

![Video tillgänglig](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur tooexpand en volym, klickar du på [här](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-hello-volume-type"></a>Ändra hello volymtyp
Du kan ändra hello volymtyp från nivåindelade toolocally Fäst eller lokalt Fäst tootiered. Den här konverteringen får inte vara en frekventa förekomst. Några skäl för att konvertera en volym från nivåindelade toolocally Fäst är:

* Lokala garantier avseende datatillgänglighet och prestanda
* Eliminering av molnet svarstiderna och molnet anslutningsproblem.

Dessa är oftast små befintliga volymer som du vill tooaccess ofta. En lokalt Fäst volym etableras fullständigt när den skapas. Om du konverterar en nivåindelad volym tooa lokalt Fäst volym, kontrollerar StorSimple att du har tillräckligt med utrymme på enheten innan den börjar hello konvertering. Om du har inte tillräckligt med utrymme du får ett felmeddelande och hello åtgärden avbryts. 

> [!NOTE]
> Innan du börjar konvertering från nivåindelade toolocally Fäst, kontrollera att du överväger hello kraven på diskutrymme på andra arbetsbelastningar. 
> 
> 

Du kanske vill toochange en lokalt Fäst volym tooa nivåindelade volymen om du behöver ytterligare utrymme tooprovision andra volymer. När du konverterar hello lokalt Fäst volym tootiered ökar hello tillgänglig kapacitet på hello enheten med hello storleken på hello publicerat kapacitet. Om problem med nätverksanslutningen förhindra hello konverteringen av en volym från hello lokala typen toohello nivåer typ, hello lokal volym kommer att ha egenskaperna för en nivåindelad volym tills hello konverteringen har slutförts. Det beror på att vissa data kan ha hamnat toohello moln. Den här spilled data fortsätter toooccupy lokalt utrymme på hello-enhet som inte går att frigöra tills hello åtgärden startas om och slutförs.

> [!NOTE]
> Konvertera en volym kan ta lite tid och du kan inte avbryta en konvertering när den har startat. hello volymen är online under hello konverteringen, och du kan göra säkerhetskopior, men du kan inte expandera eller återställa hello volym medan hello konverteringen sker.  
> 
> 

Konverteringen från en skiktad tooa lokalt Fäst volym kan Enhetsprestanda försämras. Hello följande faktorer kan dessutom öka hello tid det tar toocomplete hello konvertering:

* Det finns inte tillräckligt bandbredd.
* Det finns ingen aktuell säkerhetskopia.

toominimize hello effekter som kan ha följande faktorer:

* Granska dina bandbreddsbegränsning principer och se till att det finns en dedikerad 40 Mbit/s bandbredd.
* Schemalägga hello konvertering för låg belastning.
* Ta en ögonblicksbild i molnet innan du börjar hello konvertering.

Om du konverterar flera volymer (stöd för olika arbetsbelastningar) bör du prioritera hello Volymkonvertering så att den högre prioritet volymer konverteras först. Du bör till exempel konvertera volymer som är värdar för virtuella datorer (VM) eller volymer med SQL arbetsbelastningar innan du konverterar volymer med filen resursen arbetsbelastningar.

#### <a name="toochange-hello-volume-type"></a>toochange hello volymtyp
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj en volymbehållare hello listan och dubbelklicka på den tooview hello volymer som är associerade med hello-behållare.
3. Välj en volym och hello längst hello-sidan, klickar du på **ändra**. hello startas ändra volym.
4. På hello **grundläggande inställningar** ändrar hello användningstyp genom att välja hello nya typen hello **användningstyp** listrutan.
   
   * Om du ändrar hello typ för**lokalt Fäst**, StorSimple kontrollerar toosee om det finns tillräckligt med kapacitet.
   * Om du ändrar hello typ för**skiktindelade** och kommer att användas volymen för arkiveringsdata, markerar hello **Använd volymen för mindre ofta använda arkiveringsdata** kryssrutan.
     
       ![Kryssruta för arkivering](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)
5. Klicka på pilikonen hello ![pilikonen](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) toogo toohello **ytterligare inställningar** sidan. Om du konfigurerar en lokalt Fäst volym, visas hello följande meddelande.
   
    ![Ändra volym typen meddelande](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)
6. Klicka på pilikonen hello ![pilikon](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) toocontinue igen.
7. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) toostart hello konverteringen. hello Azure-portalen visas ett meddelande om uppdatering volym. Ett meddelande visas när hello volymen har uppdaterats.

## <a name="take-a-volume-offline"></a>Koppla från en volym
Du kanske måste tootake en volym offline när du planerar toomodify den eller ta bort den. När en volym är offline, är den inte tillgänglig för skrivskyddad åtkomst. Du behöver tootake hello volymen offline på värden för hello samt på hello enhet. 

#### <a name="tootake-a-volume-offline"></a>tootake en volym som är offline
1. Kontrollera att hello volymen i fråga inte används innan den tas offline.
2. Ta hello volymen offline på värden för hello första. Detta tar bort alla potentiella risken för korrupta data på hello volym. Specifika anvisningar finns i toohello instruktioner för operativsystemet för värden.
3. Efter hello värden är offline, ta hello volymen på hello enhet offline genom att utföra hello följande steg:
   
   1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken hello **Volymbehållare** fliken listor i tabellformat alla Hej volymbehållare som är associerade med hello enhet.
   2. Välj en volymbehållare och på den toodisplay hello lista över alla hello volymer i hello behållaren.
   3. Välj en volym och klicka på **ta offline**.
   4. Klicka på **Ja** när du uppmanas att bekräfta åtgärden. hello volymen bör nu vara offline.
      
      När en volym är offline, hello **Anslut** alternativet blir tillgängligt.

> [!NOTE]
> Hej **ta Offline** kommando skickar en begäran toohello enhet tootake hello volym offline. Om värdar fortfarande använder hello volymen, detta resulterar i avbrutna anslutningar, men inte koppla hello volymen misslyckas. 
> 
> 

## <a name="delete-a-volume"></a>Ta bort en volym
> [!IMPORTANT]
> Du kan ta bort en volym endast om den är offline.
> 
> 

Slutför följande steg toodelete en volym hello.

#### <a name="toodelete-a-volume"></a>toodelete en volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj hello volymbehållare som har hello volym toodelete. Klicka på hello volym behållaren tooaccess hello **volymer** sidan.
3. Alla hello volymer som är associerade med den här behållaren visas i tabellformat. Kontrollera status för hello hello volym som du vill toodelete. Om hello volymen som du vill toodelete inte är offline kan försättas offline första, följande hello steg [kopplar från en volym](#take-a-volume-offline).
4. När hello volymen är offline, klickar du på **ta bort** på hello hello sidans nederkant.
5. Klicka på **Ja** när du uppmanas att bekräfta åtgärden. hello volymen tas bort och hello **volymer** sidan visar hello uppdaterad lista över volymer i hello behållaren.
   
   > [!NOTE]
   > Om du tar bort en lokalt Fäst volym uppdateras hello diskutrymme för nya volymer inte omedelbart. Hej StorSimple Manager-tjänsten uppdaterar regelbundet hello lokalt tillgängligt utrymme. Vi rekommenderar att du vänta några minuter innan du försöker toocreate hello ny volym.<br> Om du tar bort en lokalt Fäst volym och ta bort en annan lokalt Fäst dessutom volymen omedelbart efteråt hello volym jobb körs sekventiellt. hello jobbet första volym måste slutföras innan hello jobbet nästa volym kan börja.
   > 
   > 

## <a name="monitor-a-volume"></a>Övervaka en volym
Övervakning av volym kan du toocollect I/O-relaterade statistik för en volym. Övervakning är aktiverad som standard för hello första 32 volymer som du skapar. Övervakning av ytterligare volymer är inaktiverat som standard. Övervakning av klonade volymer är också inaktiverat som standard.

Utför följande steg tooenable hello eller inaktivera övervakning för en volym.

#### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable eller inaktivera övervakning av volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj hello volymbehållare i vilken hello volymen finns och klicka sedan på hello volym behållaren tooaccess hello **volymer** sidan.
3. Alla hello volymer som är associerade med den här behållaren visas i hello tabular visas. Klicka och välj hello volym eller volym kloning.
4. Hello längst hello-sidan, klickar du på **ändra**.
5. I guiden Ändra volym hello under **grundläggande inställningar**väljer **aktivera** eller **inaktivera** från hello **övervakning** listrutan.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[klona en StorSimple-volym](storsimple-clone-volume.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

