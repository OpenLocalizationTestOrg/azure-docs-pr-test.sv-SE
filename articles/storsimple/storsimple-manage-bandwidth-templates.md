---
title: aaaManage StorSimple bandbredd mallar | Microsoft Docs
description: "Beskriver hur toomanage StorSimple bandbredd mallar, vilket gör att du toocontrol bandbreddsanvändning."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0027b90e-91a5-437d-9bd0-06b05674aa5f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: 3f767e985667121e977106e7a1f8e5a3ad25f022
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-bandwidth-templates"></a>Använd hello StorSimple Manager-tjänsten toomanage StorSimple bandbredd mallar
## <a name="overview"></a>Översikt
Bandbredd-mallar kan du tooconfigure nätverkets bandbredd över flera tid på dagen scheman tootier hello data från hello StorSimple enhet toohello moln.

Med bandbreddsbegränsning scheman kan du:

* Ange anpassade bandbredd scheman beroende på hello arbetsbelastning nätverket användningsområden.
* Centralisera hanteringen och återanvända hello scheman över flera enheter på ett enkelt och smidigt sätt.

> [!NOTE]
> Den här funktionen är tillgänglig endast för fysiska StorSimple-enheter och inte för virtuella enheter.
> 
> 

Alla mallar för hello bandbredd för din tjänst visas i tabellformat och innehålla hello följande information:

* **Namnet** – ett unikt namn som tilldelats toohello bandbredd mallen när den skapades.
* **Schemat** – hello antalet scheman som finns i en viss bandbreddsmall.
* **Används av** – hello antalet volymer med hello bandbredd mallar.

Du använder hello StorSimple Manager-tjänsten **konfigurera** sida i hello Azure klassiska portal toomanage bandbredd mallar.

Du kan också hitta ytterligare information toohelp konfigurera bandbredd mallar i:

* Frågor och svar om bandbredd mallar
* Metodtips för bandbredd mallar

## <a name="add-a-bandwidth-template"></a>Lägg till en bandbreddsmall
Utför följande steg toocreate en ny bandbreddsmall för hello.

#### <a name="tooadd-a-bandwidth-template"></a>tooadd en bandbreddsmall
1. På hello StorSimple Manager-tjänsten **konfigurera** klickar du på **lägga till eller redigera bandbreddsmall**.
2. I hello **Lägg till/redigera Bandbreddsmall** dialogrutan:
   
   1. Från hello **mallen** listrutan, Välj **Skapa nytt** tooadd en ny bandbreddsmall för.
   2. Ange ett unikt namn för mallen för bandbredd.
3. Definiera en **bandbredd schema**. toocreate ett schema:
   
   1. Välj hello dagarnas hello veckoschema hello har konfigurerats för hello nedrullningsbara listan. Du kan välja flera dagar genom att markera kryssrutorna för hello före hello respektive dagar i hello lista.
   2. Välj hello **hela dagen** om hello schema tillämpas för hello hela dagen. När det här alternativet är markerat kan du inte längre ange en **starttid** eller en **sluttid**. hello schema körs från 12:00 AM too11: 59 PM.
   3. Hello listrutan, Välj en **starttid**. Detta är när hello schema börjar.
   4. Hello listrutan, Välj en **sluttid**. Detta är när hello schema stoppas.
      
      > [!NOTE]
      > Överlappande scheman tillåts inte. Om hello start- och sluttider resulterar i ett överlappande schema, visas en fel meddelande toothat effekt.
      > 
      > 
   5. Ange hello **bandbredd hastighet**. Detta är hello bandbredd i megabit per sekund (Mbps) som används av din StorSimple-enhet i åtgärder som rör hello molnet (överföringar och hämtningsbara filer). Ange ett tal mellan 1 och 1000 för det här fältet.
   6. Klicka på kryssikonen hello ![kryssikonen](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). hello-mallen som du har skapat läggs toohello lista över mallar för bandbredd på hello service **konfigurera** sidan.
      
      ![Skapa en ny bandbreddsmall](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)
4. Klicka på **spara** längst hello hello sidan och klicka sedan på **Ja** när du uppmanas att bekräfta. Hello konfigurationsändringar som du har gjort sparas.

## <a name="edit-a-bandwidth-template"></a>Redigera en bandbreddsmall
Utför följande steg tooedit en bandbreddsmall hello.

### <a name="tooedit-a-bandwidth-template"></a>tooedit en bandbreddsmall
1. Klicka på **lägga till eller redigera bandbreddsmall**.
2. I hello **Lägg till/redigera Bandbreddsmall** dialogrutan:
   
   1. Från hello **mallen** listrutan väljer du en befintlig bandbredd mall du vill använda toomodify.
   2. Slutför ändringarna. (Du kan ändra de befintliga inställningarna för hello.)
   3. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Hello ändrade mallen i hello lista över bandbredd mallar visas på konfigurationssidan för hello-tjänsten.
3. toosave dina ändringar klickar du på **spara** på hello hello sidans nederkant. Klicka på **Ja** när du uppmanas att bekräfta.

> [!NOTE]
> Du kommer inte toosave ändringarna om hello redigerade schemat överlappar ett befintligt schemalägga i hello bandbreddsmall som du ändrar.
> 
> 

## <a name="delete-a-bandwidth-template"></a>Ta bort en bandbreddsmall
Utför följande steg toodelete en bandbreddsmall hello.

#### <a name="toodelete-a-bandwidth-template"></a>toodelete en bandbreddsmall
1. Välj hello-mallen som du vill toodelete i hello tabular lista hello bandbredd mallar för din tjänst. En borttagningsikonen (**x**) visas toohello extremt höger av hello valda mallen. Klicka på hello **x** ikonen toodelete hello mallen.
2. Du uppmanas att bekräfta. Klicka på **OK** tooproceed.

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

**EN**. Anta att du har en enhet med 3 volymbehållare. hello scheman som är associerade med de här behållarna helt överlappar varandra. För var och en av de här behållarna är hello bandbreddsgränser som används 5, 10, och 15 Mbit/s. När I/o som utförs på alla dessa behållare på hello samtidigt, hello minst hello 3 bandbreddsgränser kan användas: i det här fallet 5 Mbit/s som dessa utgående i/o-begäranden resursen hello samma kö.

## <a name="best-practices-for-bandwidth-templates"></a>Metodtips för bandbredd mallar
Följ dessa rekommenderade säkerhetsmetoderna för din StorSimple-enhet:

* Konfigurera bandbredd mallar på din enhet tooenable variabeln begränsning av hello dataflödet i nätverket av hello enheten vid olika tidpunkter på dagen hello. Mallarna bandbredd när det används med scheman för säkerhetskopiering kan effektivt utnyttja nätverkets bandbredd för molnet åtgärder vid låg belastning.
* Beräkna hello faktiska bandbredd som krävs för en viss distribution som baseras på hello storleken på hello distribution och hello krävs recovery tid mål för Återställningstid.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

