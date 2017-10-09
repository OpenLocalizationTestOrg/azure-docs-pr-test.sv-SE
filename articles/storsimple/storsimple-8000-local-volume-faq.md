---
title: "aaaStorSimple lokalt fästa volymer vanliga frågor och svar | Microsoft Docs"
description: "Ger svar toofrequently och frågor om StorSimple lokalt fästa volymer."
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2017
ms.author: manuaery
ms.openlocfilehash: a3a6557ca15e7e1947b45dcfd005640103c09591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple lokalt fästa volymer: vanliga frågor (FAQ)
## <a name="overview"></a>Översikt
hello följande är frågor och svar som kan uppstå när du skapar en StorSimple lokalt Fäst volym, konvertera en nivåindelad volym tooa lokalt Fäst volym (och vice versa), eller säkerhetskopiera och återställa en lokalt Fäst volym.

Frågor och svar är ordnade i följande kategorier hello

* Skapa en lokalt Fäst volym
* Säkerhetskopiera en lokalt Fäst
* Konverterar en nivåindelad volym tooa lokalt Fäst volym
* Återställa en lokalt Fäst volym
* En lokalt Fäst volym inte körs

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Frågor om hur du skapar en lokalt Fäst volym
**F.** Vad är hello maximal storlek för en lokalt Fäst volym som jag kan skapa på hello 8000-serien enheter?

**En** på enheter som kör StorSimple 8000 Series uppdatering 3.0 kan du etablera lokalt fästa volymer upp too8.5 TB, eller nivåindelade volymer upp too200 TB på 8100 hello-enhet. Du kan etablera lokalt fästa volymer upp too22.5 TB, eller nivåindelade volymer upp too500 TB på hello större 8600-enheten.

**F.** Jag har nyligen uppgraderat min 8100-enhet tooUpdate 3.0 och när jag försöker toocreate en lokalt Fäst volym hello tillgängliga maxstorleken är endast 6 TB och inte 8.5 TB. Varför kan jag skapa en volym med 8.5 TB?

**En** om enheten kör uppdatering 3.0, du kan etablera lokalt fästa volymer upp too8.5 TB eller nivåindelade volymer upp too200 TB på hello 8100-enhet. Om enheten redan har nivåindelade volymer att hello diskutrymme för att skapa en lokalt Fäst volym vara proportionerligt lägre än den här maxgränsen. Om exempelvis cirka 106 TB nivåindelade volymer har redan etablerats på din 8100-enhet (vilket är hälften av hello skikt kapacitet), och sedan hello maximal storlek för en lokal volym som du kan skapa på hello 8100-enhet kommer att motsvarande förminskad too4 TB ( ungefär Fäst hälften av hello maximalt lokalt volymkapacitet).

Eftersom vissa lokalt utrymme på enheten hello används toohost hello arbetsminne nivåindelade volymer, minskas hello tillgängligt utrymme för att skapa en lokalt Fäst volym om hello enheten har nivåindelade volymer. Däremot minskar skapa en lokalt Fäst volym proportionerligt hello tillgängligt utrymme för nivåindelade volymer. hello följande tabeller sammanfattar hello tillgänglig nivåindelade kapacitet på hello 8100 och 8600-enheter när lokalt fästa volymer skapas.

#### <a name="update-30"></a>Uppdatera 3.0 
| Lokalt fästa volymer etablerad kapacitet | Tillgänglig kapacitet toobe försetts nivåindelade volymer - 8100 | Tillgänglig kapacitet toobe försetts nivåindelade volymer - 8600 |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176.5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8.5 TB |0 TB |311.1 TB |
| 10 TB |Ej tillämpligt |277.8 TB |
| 15 TB |Ej tillämpligt |166.7 TB |
| 22,5 TB |Ej tillämpligt |0 TB |

**F.** Varför är lokalt Fäst volym skapas en långvarig åtgärd?

**S.** Lokalt fästa volymer etableras tjockt, vilket. toocreate utrymme på Hej lokala nivåerna för hello enhet, vissa data från befintliga nivåindelade volymer kan pushas toohello molnet under hello etableringsprocessen. Och eftersom detta beror på hello volym håller på att etableras, hello befintliga data på enheten och hello bandbredd toohello molnet hello storlek, hello åtgången toocreate en lokal volym kanske flera timmar.

**F.** Hur lång tid tar det toocreate en lokalt Fäst volym?

**S.** Eftersom lokalt fästa volymer etableras tjockt, vilket, att vissa befintliga data från nivåindelade volymer pushas toohello molnet under hello etableringsprocessen. Därför hello tidsåtgång toocreate en lokalt Fäst volym beror på flera faktorer, bland annat hello storlek hello volym hello data på enheten och hello tillgänglig bandbredd. Hello tid toocreate en lokalt Fäst volym är cirka 10 minuter per terabyte data på en nyligen installerade enhet som har inga volymer. Dock kan lokala volymer ta flera timmar beroende på hello faktorer som beskrivs ovan på en enhet som används.

**F.** Jag vill toocreate en lokalt Fäst volym. Finns det några metodtips som jag behöver toobe medveten om?

**S.** Lokalt fästa volymer är lämpliga för arbetsbelastningar som kräver lokala garantier data hela tiden och är känsliga toocloud svarstider. När du överväger användning av lokala volymer för någon av dina arbetsbelastningar, Tänk på följande hello:

* Lokalt fästa volymer etableras tjockt, vilket och skapa lokala volymer påverkar hello tillgängligt utrymme för nivåindelade volymer. Därför kan rekommenderar vi att du börjar med mindre volymer och skala upp när din lagring krav ökar.
* Etablering av lokala volymer är en tidskrävande åtgärd som kan handla om push-överföring av befintliga data från nivåindelade volymer toohello molnet. Därför kan det uppstå nedsatt prestanda på volymerna.
* Etablering av lokala volymer är en tidskrävande åtgärd. hello faktiska tiden som är beroende av flera faktorer: hello storleken på hello volym håller på att etableras, data på din enhet och tillgänglig bandbredd. Om du inte har säkerhetskopierat dina befintliga volymer toohello molnet är volymer skapas långsammare. Vi rekommenderar att du skapa molnögonblicksbilder av din befintliga volymer innan du etablerar en lokal volym.
* Du kan konvertera befintliga nivåindelade volymer toolocally Fäst volymer och konverteringen innefattar etablering av utrymme på hello enhet för hello resulterande lokalt Fäst volym (i tillägg toobringing nivåindelade data från hello moln). Igen, det här är en tidskrävande process som beror på faktorer som vi nämnt ovan. Vi rekommenderar att du säkerhetskopierar din befintliga volymer tidigare tooconversion som hello processen kommer att även långsammare om befintliga volymer inte som säkerhetskopieras. Enheten kan även uppstå nedsatt prestanda under den här processen.

Mer information om hur för[skapar en lokalt Fäst volym](storsimple-8000-manage-volumes-u2.md#add-a-volume)

**F.** Kan jag skapa flera lokalt fästa volymer på hello samtidigt?

**S.** Ja, men alla lokalt Fäst volym skapande och utvidgning jobb tur och ordning.

Lokalt fästa volymer etableras tjockt, vilket vilket kräver skapandet av lokalt utrymme på hello-enhet (vilket kan resultera i befintliga data från nivåindelade volymer toobe pushas toohello molnet under etableringsprocessen hello). Därför om en Etableringsjobbet pågår andra jobb för skapande av lokal volym placeras i kö tills jobbet har slutförts.

På liknande sätt, om en befintlig lokal volym utökas eller en nivåindelad volym konverteras tooa lokalt Fäst volym och hello skapandet av en ny lokalt Fäst volym är i kö tills hello tidigare jobbet har slutförts. Expandera hello storleken på en lokalt Fäst volym omfattar hello expansion hello befintliga lokala utrymme för den volymen. Konverteringen från en skiktad toolocally Fäst volym omfattar också hello skapandet av lokalt utrymme för hello resulterande lokalt Fäst volym. I båda dessa åtgärder, skapas eller expandering av lokalt utrymme lång körs jobbet.

Du kan visa dessa jobb i hello **jobb** bladet för hello StorSimple enheten Manager-tjänsten. hello jobbet som bearbetas aktivt är kontinuerligt uppdateras tooreflect hello förloppet för etablering av utrymme. hello återstående lokalt Fäst volym jobb är markerad som körs, men framstegen har stoppats och de plockats i hello ordning som de har ställts i kö.

**F.** Jag har tagit bort en lokalt Fäst volym. Varför ser jag hello frigöras utrymme återspeglas i hello tillgängligt utrymme när jag försöker toocreate en ny volym?

**S.** Om du tar bort en lokalt Fäst volym uppdateras hello diskutrymme för nya volymer inte omedelbart. Hej StorSimple Enhetshanteraren Service uppdaterar hello lokalt tillgängligt utrymme cirka varje timme. Vi rekommenderar att du väntar en timme innan du försöker toocreate hello ny volym.

**F.** Stöds lokalt fästa volymer på hello moln-enhet?

**S.** Lokalt fästa volymer stöds inte på hello moln-enhet (8010 och 8020 enheter tidigare hänvisade tooas hello virtuell StorSimple-enhet).

**F.** Kan jag använda hello Azure PowerShell-cmdlets toocreate och hantera lokalt fästa volymer?

**S.** Nej, du kan inte skapa lokalt fästa volymer via Azure PowerShell-cmdlets (alla volymer som du skapar via Azure PowerShell skikt). Även föreslår vi att du inte använder hello Azure PowerShell-cmdlets toomodify alla egenskaper för en lokalt Fäst volym när den har hello områdena leda till att hello volym typen tootiered.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Frågor om hur du säkerhetskopierar en lokalt Fäst volym
**F.** Finns lokala ögonblicksbilder av lokalt fästa volymer stöds?

**S.** Ja, kan du skapa lokala ögonblicksbilder av din lokalt fästa volymer. Men rekommenderar vi starkt att du regelbundet säkerhetskopierar din lokalt fästa volymer med molnet ögonblicksbilder tooensure som dina data skyddas i hello situation för en katastrofåterställning.

Observera att lokala ögonblicksbilder av lokalt fästa volymer kan också tjänstnivån ut toohello molnet och är inte garanterat toostay i hello lokala nivån av hello enhet.

**F.** Finns det några riktlinjer för att hantera lokala ögonblicksbilder för lokalt fästa volymer?

**S.** Frekventa lokala ögonblicksbilder tillsammans med en hög andel dataomsättningen i hello lokalt Fäst volym kan orsaka lokalt utrymme på hello enheten toobe förbrukas snabbt och resultera i att data från nivåindelade volymer som pushas toohello moln. Vi rekommenderar därför att du minimera hello antal lokala ögonblicksbilder.

**F.** Jag har fått en varning om att min lokala ögonblicksbilder av lokalt fästa volymer kan vara ogiltiga. När kan detta inträffa?

**S.** Frekventa lokala ögonblicksbilder tillsammans med en hög andel dataomsättningen i hello lokalt Fäst volym orsaka lokalt utrymme på hello enheten toobe förbrukas snabbt. Om kraftigt hello lokala nivåerna för hello enheten används ett avbrott för utökade molnet kan resultera i hello enheten blir fulla och inkommande skrivningar toohello volymen kan resultera i ogiltigförklaring av hello ögonblicksbilder (eftersom inget utrymme finns tooupdate hello ögonblicksbilder toorefer toohello äldre datablock som har skrivits över). I sådana en situation hello fortsätter skrivningar toohello volym toobe hanteras, men hello lokala ögonblicksbilder kan vara ogiltig. Det finns ingen inverkan tooyour befintliga molnögonblicksbilder.

hello varning varning är toonotify du att en sådan situation kan uppstå och se till att du lösa hello samma inom rimlig tid genom att granska din lokala ögonblicksbilder schemalägger tootake mindre ofta lokala ögonblicksbilder eller ta bort äldre lokala ögonblicksbilder som inte längre krävs.

Om hello lokala ögonblicksbilder är ogiltiga får du en avisering för information som får ett meddelande om att hello lokala ögonblicksbilder för hello specifika säkerhetskopieringsprincip har ogiltigförklarats tillsammans med hello lista över tidsstämplar hello lokala ögonblicksbilder upphävdes. Dessa ögonblicksbilder tas automatiskt bort och du kommer inte längre att kunna tooview dem i hello **säkerhetskopiering kataloger** bladet i hello Azure-portalen.

## <a name="questions-about-converting-a-tiered-volume-tooa-locally-pinned-volume"></a>Frågor om hur du konverterar en nivåindelad volym tooa lokalt Fäst volym
**F.** Jag sett vissa långsamt arbete på hello enhet vid konvertering av en nivåindelad volym tooa lokalt Fäst volym. Varför är det fortfarande händer?

**S.** hello konverteringsprocessen omfattar två steg:

1. Etablering av utrymme på hello enhet för hello Fäst snart-till--konverteras lokalt volym.
2. Hämtar alla nivåindelade data från hello molnet tooensure lokala garanterar.

Båda stegen är långa kör åtgärder som är beroende av hello storleken på hello volym som konverteras data på enheten hello och tillgänglig bandbredd. Eftersom vissa data från befintliga nivåindelade volymer kan läcker över toohello moln som en del av hello etableringsprocessen, enheten kan det uppstå nedsatt prestanda under denna tid. Dessutom hello processen kan ta längre tid om:

* Befintliga volymer har inte säkerhetskopierats toohello moln; så vi rekommenderar att säkerhetskopierar du dina volymer tidigare tooinitiating en konvertering.
* Bandbredd bandbreddsbegränsning principer har tillämpats, vilket kan begränsa hello bandbredd toohello moln; Vi rekommenderar därför att du har en dedikerad 40 Mbit/s eller mer anslutning toohello moln.
* hello processen kan ta flera timmar på grund av toohello flera faktorer som beskrivs ovan. Därför föreslår vi att du utför den här åtgärden under icke toppar gånger eller på en helgens tooavoid hello inverkan på slutet konsumenter.

Mer information om hur för[konvertera en nivåindelad volym tooa lokalt Fäst volym](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)

**F.** Kan jag avbryta hello volym konvertering?

**S.** Nej, det går inte att du hello Avbryt hello konvertering initierade en gång. Som beskrivs i hello föregående fråga kan du vara medveten om hello sämre prestanda som du kan stöta på under hello processen och följ hello bästa praxis som anges ovan när du planerar din konvertering.

**F.** Vad händer toomy volym om hello konvertering misslyckas?

**S.** Volymen konverteringen kan misslyckas på grund av problem med nätverksanslutningen toocloud. hello enheten slutar slutligen hello konverteringen efter ett antal misslyckade försök toobring nivåindelade data från hello molnet. I ett sådant scenario hello volymtyp fortsätter toobe hello källa volym typen tidigare tooconversion, och:

* En kritisk varning kommer att upphöjt toonotify du hello Konverteringsfel för volymen. Mer information om [aviseringar relaterade toolocally Fäst volymer](storsimple-8000-manage-alerts.md#locally-pinned-volume-alerts)
* Om du konverterar en skiktad tooa lokalt Fäst volym, fortsätter tooexhibit egenskaperna för en nivåindelad volym hello volymen som data kan fortfarande finnas på hello molnet. Vi rekommenderar att du löser problem med nätverksanslutningen hello och försök sedan hello konvertering.
* Vid konvertering från en lokalt Fäst tooa nivåer volymen misslyckas, även om hello volymen kommer att markeras som en lokalt Fäst volym, fungerar det på samma sätt som en nivåindelad volym (eftersom data kan ha hamnat toohello moln). Den fortsätter dock toooccupy utrymme på hello lokala nivåerna för hello enhet. Här är inte tillgänglig för andra lokalt fästa volymer. Vi rekommenderar att du gör om den här åtgärden tooensure att hello volym konverteringen har slutförts och hello lokalt utrymme på hello enhet kan vara frigöras.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Frågor om hur du återställer en lokalt Fäst volym
**F.** Är lokalt fästa volymer återställs direkt?

**S.** Ja, lokalt fästa volymer återställs omedelbart. Så snart hello metadatainformation för hello volym hämtas från hello moln som en del av återställningen hello, hello volymen är online och kan nås av hello-värden. Dock nedsatt lokala garantier för hello volym data visas inte förrän alla hello data har hämtats från hello molnet och kan uppstå prestanda på dessa volymer för hello varaktighet hello återställning.

**F.** Hur lång tid tar det toorestore en lokalt Fäst volym?

**S.** Lokalt fästa volymer återställs omedelbart och anslutas så snart hello volyminformation metadata hämtas från hello molnet, medan hello volymdata fortsätter toobe hämtas i bakgrunden för hello. Den här senare delen av hello återställningen--hämta hello lokala garantier för hello volymdata--är en tidskrävande åtgärd och kan ta flera timmar för alla hello data toobe gjorde lokala igen. hello tidsåtgång toocomplete hello samma beror på flera faktorer, till exempel hello storleken på hello volym återställs och hello tillgänglig bandbredd. Om hello ursprungliga volymen återställs har tagits bort, tas extra tid toocreate hello lokalt utrymme på hello enhet som en del av hello återställningen igen.

**F.** Jag behöver toorestore Mina befintliga lokalt Fäst volym tooan äldre ögonblicksbild (tas när hello volymen har nivåer). Hello volym återställs som nivåer i det här fallet?

**S.** Nej, återställs hello volymen som en lokalt Fäst volym. Även om hello ögonblicksbild datum toohello tid när hello volymen har nivåer när befintliga volymer använder StorSimple alltid hello typ av volymen på hello disken eftersom den finns för närvarande.

**F.** Det utökade min lokalt Fäst volym nyligen, men jag behöver nu toorestore hello data tooa tid när hello var mindre storlek. Återställ storleken hello volym och jag behöver tooextend hello storleken på hello volym när hello återställning har slutförts?

**S.** Ja, hello återställning ändrar storlek hello volym och behöver tooextend hello storleken på hello volym när hello återställning har slutförts.

**F.** Kan jag ändra hello typ av en volym under återställning?

**A.**Nej, du kan inte ändra hello volymtyp under återställning.

* Volymer som har tagits bort återställs som hello-typ som lagras i hello ögonblicksbild.
* Befintliga volymer återställs baserat på aktuell typ, oavsett hello typen lagras i hello ögonblicksbild (se toohello föregående två frågor).

**F.** Jag behöver toorestore min lokalt Fäst volym, men jag importerade en felaktig punkt i ögonblicksbilder. Kan jag avbryta hello återställningen?

**S.** Ja, du kan avbryta en pågående återställning. hello status för hello volym återställs toohello tillstånd hello början av hello återställning. Dock försvinner alla skrivningar som har gjorts toohello volym när hello återställning pågick.

**F.** Jag startade en återställning på en av min lokalt fästa volymer och nu visas en ögonblicksbild i min eftersläpning katalogen som jag inte recollect skapar. Vad är detta används för?

**S.** Detta är hello tillfälliga ögonblicksbild som har skapats tidigare toohello återställningen och används för återställning om hello återställa avbryts eller misslyckas. Ta inte bort den här ögonblicksbilden; det tas automatiskt bort när hello återställningen är slutförd. Detta kan inträffa om din Återställningsjobbet har bara lokalt Fäst volymer eller en blandning av lokalt Fäst och nivåindelade volymer. Om hello återställningsjobbet innehåller endast nivåindelade volymer, sker inte det här beteendet.

**F.** Kan jag klona en lokalt Fäst volym?

**S.** Ja, du kan. Dock kommer hello lokalt Fäst volym att klonas när en nivåindelad volym som standard. Mer information om hur för[klona en lokalt Fäst volym](storsimple-8000-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Frågor om inte körs en lokalt Fäst volym
**F.** Jag behöver toofail över min enhet tooanother fysisk enhet. Kommer min lokalt fästa volymer att växlas över som lokalt Fäst eller nivåer?

**S.** hello lokalt fästa volymer har redundansväxlats lokalt Fäst om hello målenhet kör StorSimple 8000 uppdatering 3 eller senare.

Mer information om [redundans och Katastrofåterställning för lokalt fästa volymer mellan olika versioner](storsimple-8000-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**F.** Återställs lokalt fästa volymer direkt vid katastrofåterställning (DR)?

**S.** Ja, återställs lokalt fästa volymer direkt under växling vid fel. Så snart hello metadatainformation för hello volym hämtas från hello molnet som en del av hello Redundansåtgärden hello volymen är online på hello målenhet och kan nås av hello-värden. Under tiden hello volymdata toodownload fortsätter i bakgrunden hello och nedsatt prestanda på dessa volymer för hello varaktighet hello växling vid fel kan uppstå.

**F.** Visas hello redundans jobbet är slutfört, hur kan jag spåra hello förloppet för lokalt Fäst volym återställs på hello målenhet?

**S.** Under en redundansväxling markeras hello beställningsjobbet som slutförd när alla hello volymer i hello redundans uppsättning har återställts och tas online på hello målenhet omedelbart. Det inkluderar alla lokalt fästa volymer som kanske har växlats över; lokala garantier hello data kommer dock bara är tillgänglig när alla hello data för hello volymen har hämtats. Du kan följa den här förloppet för varje lokalt Fäst volym som gick över övervakning hello motsvarande jobb som skapats som en del av hello växling vid fel. Dessa enskilda jobb skapas endast för lokalt fästa volymer.

**F.** Kan jag ändra hello typ av en volym under växling vid fel?

**S.** Nej, du kan inte ändra hello volymtyp under en växling vid fel. Om du misslyckas över tooanother fysisk enhet som kör StorSimple 8000-serien uppdatering 3, hello volymer har redundansväxlats baserat på hello volymtyp som lagras i hello ögonblicksbild.

**F.** Kan jag växla över en volymbehållare med lokalt fästa volymer toohello moln-enhet?

**S.** Ja, du kan. hello lokalt fästa volymer kommer att redundansväxlas som nivåindelade volymer. Mer information om [redundans och Katastrofåterställning för lokalt fästa volymer mellan olika versioner](storsimple-8000-device-failover-disaster-recovery.md#common-considerations-for-device-failover)

