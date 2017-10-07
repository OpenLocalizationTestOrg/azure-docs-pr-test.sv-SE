---
title: aaaManage StorSimple-volymer | Microsoft Docs
description: "Förklarar hur tooadd, ändra, övervaka och ta bort StorSimple-volymer och hur tootake dem offline om det behövs."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.openlocfilehash: 8dc646261ee2ad8f2b80291518716f4de55b63f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-volumes"></a>Använda hello StorSimple Manager-tjänsten toomanage volymer
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur toouse hello toocreate för StorSimple Manager-tjänsten och hantera volymer på hello StorSimple-enhet och virtuella StorSimple-enheten.

Hej StorSimple Manager-tjänsten är en utökning av hello klassiska Azure-portalen där du kan hantera din StorSimple-lösning från ett enda webbgränssnitt. I tillägg toomanaging volymer, kan du använda hello StorSimple Manager-tjänsten toocreate och hantera StorSimple-tjänster, visa och hantera enheter, Visa aviseringar, och visa och hantera principer för säkerhetskopiering och hello säkerhetskopieringskatalogen.

> [!NOTE]
> Azure StorSimple skapa tunt allokerade volymer. Du kan inte skapa helt eller delvis etablerade volymer på ett Azure StorSimple-system.
> 
> Tunn allokering är en virtualiseringsteknik som tillgängligt lagringsutrymme visas tooexceed fysiska resurser. I stället för att reservera tillräckligt med lagringsutrymme i förväg använder Azure StorSimple tunn etablering tooallocate bara tillräckligt med utrymme toomeet aktuella kraven. hello elastisk uppbyggnad molnlagring underlättar den här metoden eftersom Azure StorSimple kan öka eller minska molnet toomeet ändra lagringsbehov.
> 
> 

## <a name="hello-volumes-page"></a>hello volymer sida
Hej **volymer** sidan kan du toomanage hello lagringsvolymer som etablerats på hello Microsoft Azure StorSimple-enheten för dina initierarna (servrarna). Den visar hello lista över volymer på StorSimple-enheten.

 ![Volymsidan](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

En volym som består av en serie med attribut:

* **Namnet** – ett beskrivande namn som måste vara unikt och hjälper till att identifiera hello volym. Det här namnet används också övervaka rapporter när du filtrerar på en viss volym.
* **Status för** – kan vara online eller offline. Om en volym om offline, men det är inte synliga tooinitiators (servrar) som tillåts åtkomst toouse hello volym.
* **Kapacitet** – anger hur stor hello volym är, som uppfattas av hello initierare (server). Kapacitet anger hello total mängd data som kan lagras av hello initierare (server). Volymer är tunt etablerad och data är deduplicerad. Detta innebär att enheten inte allokera fysiska lagringskapacitet före internt eller i hello moln enligt tooconfigured volymens kapacitet. hello volymens kapacitet är allokerade och används på begäran.
* **Typen** – hello volymtyp kan vara nivåindelade eller arkivering (undertyp av skikt)
* **Åtkomst** – anger hello initierarna (servrarna) som tillåts åtkomst toothis volym. Initierare som inte är medlemmar i åtkomstkontrollpost (ACR) som är associerad med hello volymen kan inte se hello volym.
* **Övervaka** – anger huruvida en volym som övervakas. En volym har övervakning aktiverad som standard när den skapas. Övervakning kommer dock, inaktiveras för en volym kloning. tooenable övervakning för en volym, följ instruktionerna för hello i övervakaren en volym.

hello vanligaste uppgifter som är kopplad till en volym är:

* Lägg till en volym
* Ändra en volym
* Ta bort en volym
* Koppla från en volym
* Övervaka en volym

## <a name="add-a-volume"></a>Lägg till en volym
Du [har skapat en volym](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) under distributionen av din StorSimple-lösning. Lägger till en volym är en liknande procedur.

### <a name="tooadd-a-volume"></a>tooadd en volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj en volymbehållare och klicka på hello pilen i hello motsvarande rad tooaccess hello volymer som är associerade med hello-behållare.
3. Klicka på **Lägg till** på hello hello sidans nederkant. hello guiden Lägg till en volym startar.
   
     ![Lägg till volym slutförs grundläggande inställningar](./media/storsimple-manage-volumes/AddVolume1.png)
4. I hello guiden Lägg till en volym, under **grundläggande inställningar**, hello följande:
   
   1. Ange ett **namn** för volymen.
   2. Ange hello **Etableringskapacitet** för volymen i GB eller TB. hello kapacitet måste vara mellan 1 GB och 64 TB för en fysisk enhet. hello maximal kapacitet som kan tillhandahållas för en volym på en virtuell StorSimple-enhet är 30 TB.
   3. Välj hello **användningstyp** för volymen. Om du använder hello nivåindelade volymen för arkiveringsdata, markerar hello **Använd volymen för mindre ofta använda arkiveringsdata** kryssrutan ändrar hello segmentstorleken för deduplicering för volymen too512 KB. Om du inte väljer det här alternativet använder hello motsvarande nivåindelade volymen en segmentstorlek på 64 KB. En större segmentstorlek för deduplicering kan hello enheten tooexpedite hello överföringen av stora mängder arkiveringsdata toohello moln. (Nivåindelade volymer kallades primära volymer.)
   4. Klicka på pilikonen hello ![pilikonen](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)toogo toohello **ytterligare inställningar** sidan.
      
        ![Lägg till ytterligare inställningar för volym-guiden](./media/storsimple-manage-volumes/AddVolume2.png)
5. Under **ytterligare inställningar**, lägga till en ny åtkomstkontrollpost (ACR):
   
   1. Välj en åtkomstkontrollpost (ACR) hello nedrullningsbara listan. Alternativt kan du lägga till en ny ACR. ACRs avgör vilka värdar som kan komma åt dina volymer av matchande hello värd IQN med som hello posten i listan.
   2. Vi rekommenderar att du aktiverar en standard säkerhetskopiering genom att välja hello **aktiverar en standard säkerhetskopiering för den här volymen** kryssrutan.
   3. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-volumes/HCS_CheckIcon.png) toocreate hello volym med hello angivna inställningar.

Den nya volymen är nu redo toouse.

## <a name="modify-a-volume"></a>Ändra en volym
Ändra en volym när du behöver tooexpand den eller ändra hello värdar som har åtkomst till hello volym.

> [!IMPORTANT]
> * Om du ändrar hello volymens storlek på hello enhet måste hello volymens storlek toobe ändras på hello-värden.
> * hello värden på klientsidan stegen som beskrivs här är för Windows Server 2012 (2012R2). Procedurer för Linux eller andra värdoperativsystem är olika. Läs tooyour värden operativsystemet anvisningar när du ändrar hello volym på en värd som kör ett annat operativsystem.
> 
> 

### <a name="toomodify-a-volume"></a>toomodify en volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken. Den här sidan visas i tabellformat hello volymbehållare som är associerade med hello enhet.
2. Välj en volymbehållare och på den toodisplay hello lista över alla hello volymer i hello behållaren.
3. På hello **volymer** , Välj en volym och klickar på **ändra**.
4. Hello ändra i guiden för volym under **grundläggande inställningar**, kan du göra hello följande:
   
   * Redigera hello **namn** och **typen** om du vill toomodify en nivåindelad volym tooan arkivering volym genom att välja hello **Använd volymen för mindre ofta använda arkiveringsdata** segmentstorleken för kryssrutan toochange hello deduplicering för volymen too512 KB.
   * Öka hello **Etableringskapacitet**. Hej **Etableringskapacitet** kan bara ökas. Det går inte att komprimera en volym när den har skapats.
     
     > [!NOTE]
     > Du kan inte ändra hello volymbehållare när den är tilldelad tooa volym.
     > 
     > 
5. Under **ytterligare inställningar**, kan du göra hello följande:
   
   * Ändra hello ACRs, förutsatt att hello volymen är offline. Om hello volymen är online, behöver du tootake det offline först. Se toohello stegen i [kopplar från en volym](#take-a-volume-offline) tidigare toomodifying hello ACR.
   * Ändra hello lista över ACRs efter hello volymen är offline.
     
     > [!NOTE]
     > Du kan inte ändra hello **aktiverar en standard säkerhetskopiering för den här volymen** alternativet för hello volym.
     > 
     > 
6. Spara dina ändringar genom att klicka på kryssikonen hello ![kryssikon](./media/storsimple-manage-volumes/HCS_CheckIcon.png). hello klassiska Azure-portalen visas ett meddelande om uppdatering volym. Ett meddelande visas när hello volymen har uppdaterats.
7. Om du expanderar en volym, utför följande steg på värddatorn Windows hello:
   
   1. Gå för**Datorhantering** ->**Diskhantering**.
   2. Högerklicka på **Diskhantering** och välj **Skanna diskar**.
   3. Välj hello volym som du har uppdaterat, högerklicka och välj sedan i hello lista över diskar, **Utöka volym**. startar guiden för hello Utöka volym. Klicka på **Nästa**.
   4. Slutför guiden för hello, acceptera hello standardvärden. När hello guiden är klar ska hello volymen visa hello ökad storlek.

![Video tillgänglig](./media/storsimple-manage-volumes/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur tooexpand en volym, klickar du på [här](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Koppla från en volym
Du kanske måste tootake en volym offline när du planerar toomodify den eller ta bort den. När en volym är offline, är den inte tillgänglig för skrivskyddad åtkomst. Du behöver tootake hello volymen offline på värden för hello samt på hello enhet. Utföra hello följande steg tootake en volym som är offline.

### <a name="tootake-a-volume-offline"></a>tootake en volym som är offline
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

### <a name="toodelete-a-volume"></a>toodelete en volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj hello volymbehållare som har hello volym toodelete. Klicka på hello volym behållaren tooaccess hello **volymer** sidan.
3. Alla hello volymer som är associerade med den här behållaren visas i tabellformat. Kontrollera status för hello hello volym som du vill toodelete. Om hello volymen som du vill toodelete inte är offline kan försättas offline första, följande hello steg [kopplar från en volym](#take-a-volume-offline).
4. När hello volymen är offline, klickar du på **ta bort** på hello hello sidans nederkant.
5. Klicka på **Ja** när du uppmanas att bekräfta åtgärden. hello volymen tas bort och hello **volymer** sidan visar hello uppdaterad lista över volymer i hello behållaren.

## <a name="monitor-a-volume"></a>Övervaka en volym
Övervakning av volym kan du toocollect I/O-relaterade statistik för en volym. Övervakning är aktiverad som standard för hello första 32 volymer som du skapar. Övervakning av ytterligare volymer är inaktiverat som standard. Övervakning av klonade volymer är också inaktiverat som standard.

Utför följande steg tooenable hello eller inaktivera övervakning för en volym.

### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable eller inaktivera övervakning av volym
1. På hello **enheter** sidan Välj hello enheten, dubbelklicka på den, och klicka sedan på hello **Volymbehållare** fliken.
2. Välj hello volymbehållare i vilken hello volymen finns och klicka sedan på hello volym behållaren tooaccess hello **volymer** sidan.
3. Alla hello volymer som är associerade med den här behållaren visas i hello tabular visas. Klicka och välj hello volym eller volym kloning.
4. Hello längst hello-sidan, klickar du på **ändra**.
5. I guiden Ändra volym hello under **grundläggande inställningar**väljer **aktivera** eller **inaktivera** från hello **övervakning** listrutan.
   
    ![Ändra en volym grundläggande inställningar](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[klona en StorSimple-volym](storsimple-clone-volume.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

