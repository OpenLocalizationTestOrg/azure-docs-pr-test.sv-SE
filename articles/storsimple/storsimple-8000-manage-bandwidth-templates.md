---
title: "aaaManage bandbredd mallar för StorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hur toomanage StorSimple bandbredd mallar, vilket gör att du toocontrol bandbreddsanvändning."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: ebcd1824d7bb9e4c235194c04edbfe8001a3e794
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-bandwidth-templates"></a>Använd hello StorSimple Enhetshanteraren service toomanage StorSimple bandbredd mallar

## <a name="overview"></a>Översikt

Bandbredd-mallar kan du tooconfigure nätverkets bandbredd över flera tid på dagen scheman tootier hello data från hello StorSimple enhet toohello moln.

Med bandbreddsbegränsning scheman kan du:

* Ange anpassade bandbredd scheman beroende på hello arbetsbelastning nätverket användningsområden.
* Centralisera hanteringen och återanvända hello scheman över flera enheter på ett enkelt och smidigt sätt.

> [!NOTE]
> Den här funktionen är tillgänglig endast för fysiska StorSimple-enheter (modeller 8100 och 8600) och inte för StorSimple moln installationer (modeller 8010 och 8020).


## <a name="hello-bandwidth-templates-blade"></a>hello bandbredd mallar bladet

Hej **bandbredd mallar** bladet har alla hello bandbredd mallar för tjänsten i tabellformat och innehåller hello följande information:

* **Namnet** – ett unikt namn som tilldelats toohello bandbredd mallen när den skapades.
* **Schemat** – hello antalet scheman som finns i en viss bandbreddsmall.
* **Används av** – hello antalet volymer med hello bandbredd mallar.

Du kan också hitta ytterligare information toohelp konfigurera bandbredd mallar i:

* [Frågor och svar om bandbredd mallar](#questions-and-answers-about-bandwidth-templates)
* [Metodtips för bandbredd mallar](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Lägg till en bandbreddsmall

Utför följande steg toocreate en ny bandbreddsmall för hello.

#### <a name="tooadd-a-bandwidth-template"></a>tooadd en bandbreddsmall

1. Gå tooyour StorSimple Enhetshanteraren service **bandbredd mallar** och klicka sedan på **+ Lägg till bandbreddsmall**.

    ![Klicka på + Lägg till bandbreddsmall](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. I hello **Lägg till bandbreddsmall** bladet hello följande steg:
   
    1. Ange ett unikt namn för mallen för bandbredd.
    2. Definiera ett schema för bandbredd. toocreate ett schema:
   
        1. Hello nedrullningsbara listan och väljer hello **dagar** av hello vecka hello schema har konfigurerats för. Du kan ange flera dagar.        
        
        2. Ange en **starttid** i _hh: mm_ format. Detta är när hello schema börjar.

        3. Ange en **sluttid** i _hh: mm_ format. Detta är när hello schema stoppas.
      
           > [!NOTE]
           > Överlappande scheman tillåts inte. Om hello start- och sluttider resulterar i ett överlappande schema, visas en fel meddelande toothat effekt.

        4. Ange hello **bandbredd hastighet**. Detta är hello bandbredd i megabit per sekund (Mbps) som används av din StorSimple-enhet i åtgärder som rör hello molnet (överföringar och hämtningsbara filer). Ange ett tal mellan 1 och 1000 för det här fältet.

            ![Definiera bandbredd schema](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            Upprepa hello senare steg toodefine flera scheman för mallen tills du är klar.

        5. Klicka på **Lägg till** toostart skapar en bandbreddsmall. hello skapat mall läggs toohello lista över mallar för bandbredd.
      

## <a name="edit-a-bandwidth-template"></a>Redigera en bandbreddsmall

Utför följande steg tooedit en bandbreddsmall hello.

### <a name="tooedit-a-bandwidth-template"></a>tooedit en bandbreddsmall

1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **bandbredd mallar**.
2. Välj hello-mallen som du vill toodelete i hello lista bandbredd mallar. Högerklicka på och hello snabbmenyn, Välj **ta bort**.
3. När du uppmanas att bekräfta, klickar du på **OK**. Det bör ta bort hello bandbreddsmall. 
4. hello lista över bandbredd mallar uppdateras tooreflect hello borttagning.

> [!NOTE]
> Du kan inte spara ändringarna om hello redigerade schemat överlappar ett befintligt schema i hello bandbreddsmall som du ändrar.

## <a name="delete-a-bandwidth-template"></a>Ta bort en bandbreddsmall

Utför följande steg toodelete en bandbreddsmall hello.

#### <a name="toodelete-a-bandwidth-template"></a>toodelete en bandbreddsmall

1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **bandbredd mallar**.
2. Välj hello-mallen som du vill toodelete i hello lista bandbredd mallar. Högerklicka och välj Ta bort hello snabbmenyn.
3. När du uppmanas att bekräfta, klickar du på **OK**. Det bör ta bort hello bandbreddsmall.
4. hello lista över bandbredd mallar uppdateras tooreflect hello borttagning.

Om hello mall är av någon volymerna, du kommer inte toodelete den. Du ser ett felmeddelande om att hello mallen används. En dialogrutan med felmeddelandet visas som talar om att alla hello referenser toohello mallen bör tas bort.

Du kan ta bort alla hello referenser toohello mallen genom att öppna hello **Volymbehållare** sidan och ändra hello volymbehållare som använder den här mallen så att de använder en annan mall eller använda en anpassad eller obegränsad bandbredd inställningen. När alla hello referenser har tagits bort kan du ta bort hello mallen.

## <a name="use-a-default-bandwidth-template"></a>Använda en standardmall för bandbredd

En standardmall för bandbredd tillhandahålls och används av volymbehållare av standardkontrollerna tooenforce bandbredd vid åtkomst till hello molnet. hello standardmallen fungerar också som en referens som är redo för användare skapar egna mallar. hello information om den här standardmallen är:

* **Namnet** – obegränsade kvällar och helger
* **Schemat** – ett enda schema från måndag tooFriday som gäller en bandbredd andel 1 Mbit/s mellan 8: 00 och 17: 00 tid. hello bandbredd är inställd tooUnlimited för hello resten av hello vecka.

hello standardmallen kan redigeras. hello användning av den här mallen (inklusive redigerade versioner) spåras.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Skapa en varar bandbreddsmall som börjar vid en angiven tid

Följ den här proceduren toocreate ett schema som startar vid en viss tidpunkt och kör hela dagen. I exemplet hello hello schema börjar vid 9: 00 hello morgonen och körs förrän 9: 00 hello nästa morgon. Det är viktigt toonote som hello start och sluttider för ett visst schema måste båda finnas på hello samma 24-timmarsformat schema och kan sträcka sig över flera dagar. Om du behöver tooset bandbredd mallar som sträcker sig över flera dagar, måste toouse flera scheman (som visas i exemplet hello).

#### <a name="toocreate-an-all-day-bandwidth-template"></a>toocreate en varar bandbreddsmall

1. Skapa ett schema som börjar vid 9: 00 hello morgonen och kör till midnatt.
2. Lägg till ett annat schema. Konfigurera hello andra schema toorun från midnatt tills 9: 00 hello morgonen.
3. Spara hello bandbreddsmall.

hello sammansatt schema kommer sedan att starta i taget väljer och kör varar.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Frågor och svar om bandbredd mallar

**Q**. Vad händer toobandwidth kontroller när du är between hello scheman? (Ett schema har avslutats och en annan har inte startat ännu.)

**EN**. I sådana fall kommer inga bandbreddskontroller användas. Detta innebär att hello-enheten kan använda obegränsad bandbredd när skiktning data toohello moln.

**Q**. Kan du ändra bandbredd mallar på en offline-enhet?

**EN**. Du kommer inte att kunna toomodify bandbredd mallar på volymer behållare om motsvarande hello-enheten är offline.

**Q**. Kan du redigera en bandbreddsmall som är associerade med en volymbehållare när hello associerade volymer är offline?

**EN**. Du kan ändra en bandbreddsmall som är associerade med en volymbehållare vars volymer som är offline. Observera att när volymer är offline, inga data inte kommer nivåindelas från hello enhet toohello moln.

**Q**. Kan du ta bort en standardmall?

**EN**. Du kan ta bort en standardmall, är det inte en bra idé toodo så. hello användning av en standardmall, inklusive redigerade versionerna spåras. hello spårningsdata analyseras och över hello förstås tid är används tooimprove hello standardmallen.

**Q**. Hur vet du att mallarna bandbredd måste toobe ändras?

**EN**. En av hello tecken som du behöver toomodify hello bandbredd mallar är när du startar ser hello nätverket långsammare eller strypning flera gånger under en dag. Om det händer kan du övervaka hello lagrings- och nätverk genom att titta på hello diagram för i/o-prestanda och dataflödet i nätverket.

Identifiera hello tid på dagen från hello för genomströmning nätverksdata och hello volymbehållare i vilken hello inträffar flaskhalsar i nätverket. Om detta händer när data håller på att nivåindelade toohello molnet (hämta informationen från i/o-prestanda för alla volymbehållare för enheten toocloud), behöver du toomodify hello bandbredd mallar som är associerade med din volymbehållare.

När hello ändrats mallar används så behöver du toomonitor hello nätverket igen för betydande fördröjningar. Om de fortfarande finns måste toorevisit bandbredd mallarna.

**Q**. Vad händer om flera volymbehållare på enheten har schemalägger som överlappar varandra men olika begränsningar gäller tooeach?

**EN**. Anta att du har en enhet med 3 volymbehållare. hello scheman som är associerade med de här behållarna helt överlappar varandra. För var och en av de här behållarna är hello bandbreddsgränser som används 5, 10, och 15 Mbit/s. När i/o uppstår på alla dessa behållare på hello tillämpas samtidigt, hello minst hello 3 bandbreddsgränser: i det här fallet 5 Mbit/s som dessa utgående i/o-begäranden resursen hello samma kö.

## <a name="best-practices-for-bandwidth-templates"></a>Metodtips för bandbredd mallar

Följ dessa rekommenderade säkerhetsmetoderna för din StorSimple-enhet:

* Konfigurera bandbredd mallar på din enhet tooenable variabeln begränsning av hello dataflödet i nätverket av hello enheten vid olika tidpunkter på dagen hello. Mallarna bandbredd när det används med scheman för säkerhetskopiering kan effektivt utnyttja nätverkets bandbredd för molnet åtgärder vid låg belastning.
* Beräkna hello faktiska bandbredd som krävs för en viss distribution som baseras på hello storleken på hello distribution och hello krävs recovery tid mål för Återställningstid.

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

