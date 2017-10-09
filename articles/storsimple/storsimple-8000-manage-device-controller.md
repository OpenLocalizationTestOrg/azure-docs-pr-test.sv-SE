---
title: aaaManage StorSimple 8000-serien styrenheter | Microsoft Docs
description: "Lär dig hur toostop, starta om, stänga av eller återställa din StorSimple-styrenheter."
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
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: 5c59582b7ccf7cfeae9e7efbd0e4df9dc1d3871c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Hantera din StorSimple-styrenheter

## <a name="overview"></a>Översikt

Den här självstudiekursen beskriver hello olika åtgärder som kan utföras på din StorSimple-styrenheter. hello-styrenheter i din StorSimple-enhet är redundant (peer) domänkontrollanter i ett aktivt-passivt konfiguration. Endast en domänkontrollant är aktiv vid en given tidpunkt och bearbetar alla hello disk- och åtgärder. hello är andra domänkontrollanter i passivt läge. Om hello aktiva styrenheten misslyckas aktiveras hello passiva domänkontrollant automatiskt.

Vägledningen innehåller stegvisa instruktioner toomanage hello-styrenheter med hjälp av den:

* **Domänkontrollanter** bladet för din enhet i hello StorSimple enheten Manager-tjänsten.
* Windows PowerShell för StorSimple.

Vi rekommenderar att du hanterar hello styrenheter via hello StorSimple enheten Manager-tjänsten. Om en åtgärd kan endast utföras med hjälp av Windows PowerShell för StorSimple, gör en del av hello kursen.

När du har läst den här självstudiekursen kommer du att kunna:

* Starta om eller stänga av en domänkontrollant för StorSimple-enhet
* Stänga av en StorSimple-enhet
* Återställ standardvärden för toofactory din StorSimple-enhet

## <a name="restart-or-shut-down-a-single-controller"></a>Starta om eller stänga av en enda domänkontrollant
En domänkontrollant omstart eller avstängning krävs inte som en del av normal drift. Avstängning åtgärder för en enskild enhet är vanliga endast i fall där en misslyckad enhet maskinvarukomponent behöver bytas ut. En omstart av domänkontrollanten kan också krävas i en situation där prestanda påverkas av omfattande minnesanvändning eller en felaktig styrenhet. Du kanske också måste toorestart en domänkontrollant efter en lyckad controller ersättning, om du vill tooenable och testa hello ersättas domänkontrollant.

Starta om en enhet är inte störande tooconnected initierare, förutsatt att hello passiva domänkontrollant är tillgänglig. Om en passiv domänkontrollant är inte tillgänglig eller inaktiverad, startar om hello active kan styrenhet resultera i hello avbrott i tjänsten och driftstopp.

> [!IMPORTANT]
> * **En domänkontrollant som körs ska aldrig tas fysiskt bort eftersom det skulle resultera i förlust av redundans och en ökad risk för driftstopp.**
> * hello gäller följande procedur bara toohello fysiska StorSimple-enheten. Information om hur toostart, stoppa och omstart hello StorSimple-enhet för molnet, se [arbeta med hello molnet installation](storsimple-8000-cloud-appliance-u2.md##work-with-the-storsimple-cloud-appliance).

Du kan starta om eller stänga av en enskild enhet domänkontrollant via hello Azure-portalen för hello StorSimple Enhetshanteraren tjänst eller Windows PowerShell för StorSimple.

toomanage din styrenheter från hello Azure-portalen utföra hello följande steg.

#### <a name="toorestart-or-shut-down-a-controller-in-azure-portal"></a>toorestart eller Stäng av en domänkontrollant i Azure-portalen
1. I Enhetshanteraren för StorSimple-tjänsten går för**enheter**. Välj enheten hello lista över enheter. 

    ![Välj en enhet](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Gå för**Inställningar > styrenheter**.
   
    ![Kontrollera StorSimple-styrenheter är felfria](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. I hello **domänkontrollanter** bladet, kontrollera att hello status för båda hello domänkontrollanter på din enhet är **felfri**. Markera en domänkontrollant, högerklicka och välj sedan **starta om** eller **Stäng**.

    ![Välj Starta om eller stänga av StorSimple-styrenheter](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Ett jobb skapas toorestart eller stänga av hello domänkontrollant och visas med tillämpliga varningar, om sådana finns. toomonitor hello omstart eller avstängning, gå för**Service > aktivitetsloggar** och filtrera sedan av parametrar specifika tooyour-tjänsten. Om en domänkontrollant stängdes, så måste toopush hello power knappen tooturn på hello controller tooturn på.

#### <a name="toorestart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>toorestart eller Stäng av en domänkontrollant i Windows PowerShell för StorSimple
Utför följande steg tooshut ned hello eller starta om en enda domänkontrollanten på StorSimple-enheten från hello Windows PowerShell för StorSimple.

1. Hello enhet via seriekonsolen hello eller en telnet-session från en fjärrdator. tooconnect tooController 0 eller 1-styrenhet, gör hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.
3. I hello Banderollmeddelandet anteckna hello-domänkontrollant som du är ansluten för (styrenhet 0 eller 1-styrenhet) och om det är hello aktiva eller passiva (standby) hello-styrenheten.
   
   * tooshut ned en enda domänkontrollant i Kommandotolken hello typ:
     
       `Stop-HcsController`
     
       Detta avslutar hello-domänkontrollant som du är ansluten till. Om du stoppar hello aktiva styrenheten misslyckas hello enheten via toohello passiva domänkontrollant.

   * toorestart skriver du en domänkontrollant i Kommandotolken hello:
     
       `Restart-HcsController`
     
       Detta startar om hello-domänkontrollant som du är ansluten till. Om du startar om hello aktiva styrenheten flyttas över toohello passiva domänkontrollant innan hello startar om.

## <a name="shut-down-a-storsimple-device"></a>Stänga av en StorSimple-enhet

Det här avsnittet beskrivs hur tooshut ned en löpande eller en misslyckad StorSimple-enhet från en fjärrdator. En enhet är avstängd när båda hello-styrenheter är stängt. En enhet avstängning görs när hello enhet fysiskt flyttas eller tas ur funktion.

> [!IMPORTANT]
> Kontrollera hello hälsa hello komponenter innan du stänger hello enhet. Navigera tooyour enheten och klicka sedan på **Inställningar > maskinvara hälsa**. I hello **Status och maskinvara hälsa** bladet, kontrollera att hello Indikator status för alla hello-komponenter är grön. Endast en felfri enhet har en grön status. Om enheten är att stänga av tooreplace en felaktig komponent, visas en misslyckad (röd) eller en försämrad (gul) status för hello respektive komponenterna.


#### <a name="tooshut-down-a-storsimple-device"></a>tooshut ned en StorSimple-enhet

1. Använd hello [starta om eller stänga av en domänkontrollant](#restart-or-shut-down-a-single-controller) proceduren tooidentify och avsluta hello passiva domänkontrollant på enheten. Du kan utföra den här åtgärden i hello Azure-portalen eller i Windows PowerShell för StorSimple.
2. Upprepa hello senare steg tooshut ned hello aktiva styrenhet.
3. Du måste nu tittar på hello tillbaka plan för hello enhet. När hello två domänkontrollanter helt är stängt ska hello status indikatorer på båda hello domänkontrollanter blinkande red. Om du behöver tooturn av hello enheten helt just nu Vänd hello power växlar på både ström och kylning moduler (PCMs) toohello OFF-läge. Detta ska stänga av hello enhet.

## <a name="reset-hello-device-toofactory-default-settings"></a>Återställ standardinställningar för hello enheten toofactory

> [!IMPORTANT]
> Kontakta Microsoft Support om du behöver tooreset toofactory standardinställningarna på enheten. hello proceduren som beskrivs nedan bör endast användas tillsammans med Microsoft-supporten.

Den här proceduren beskriver hur tooreset din Microsoft Azure StorSimple enheten toofactory standardinställningar med hjälp av Windows PowerShell för StorSimple.
När du återställer en enhet tar bort alla data och inställningar från hello hela klustret som standard.

Utför följande steg tooreset hello standardinställningarna för Microsoft Azure StorSimple-enhet toofactory:

### <a name="tooreset-hello-device-toodefault-settings-in-windows-powershell-for-storsimple"></a>tooreset hello toodefault Enhetsinställningar i Windows PowerShell för StorSimple
1. Hello enhet via dess seriekonsol. Kontrollera att du är ansluten toohello hello banderoll meddelandet tooensure **Active** domänkontrollant.
2. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.
3. I hello kommandotolk, skriver du hello efter kommandot tooreset hello hela klustret, ta bort alla data och metadata för domänkontrollanten inställningar:
   
    `Reset-HcsFactoryDefault`
   
    tooinstead återställa en enda domänkontrollant använder hello [Återställ HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) med hello `-scope` parameter.)
   
    hello systemet startas om flera gånger. Du meddelas när hello återställning har slutförts. Det kan ta den här processen 45 – 60 minuter under en 8100-enhet och 60-90 minuter för en 8600 toofinish beroende på hello systemmodell.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Frågor och svar om hur du hanterar styrenheter
I det här avsnittet har vi sammanfattas några av hello vanliga frågor angående hantera StorSimple-styrenheter.

**F.** Vad händer om båda hello domänkontrollanter på enheten är felfri och aktiverade och starta om eller stänga hello aktiva styrenheten?

**S.** Om båda hello domänkontrollanter på din enhet är felfri och aktiverade på, du uppmanas att bekräfta. Du kan välja att:

* **Starta om hello aktiva styrenheten** – du meddelas att starta om en domänkontrollant för active orsakade hello enheten toofail över toohello passiva domänkontrollant. hello domänkontrollanten startar om.
* **Stänga av en domänkontrollant för en aktiv** – du meddelas att stänga av en domänkontrollant för en aktiv resulterar i driftstopp. Du måste också toopush hello strömknappen på hello enheten tooturn på hello-domänkontrollant.

**F.** Vad händer om hello passiva domänkontrollant på enheten är inte tillgänglig eller aktiverade inaktiverat och jag startar om eller stänger hello aktiva styrenheten?

**S.** Om hello passiva domänkontrollanten på din enhet är tillgänglig eller så är aktiverade inaktiverat och du väljer att:

* **Starta om hello aktiva styrenheten** – du meddelas att fortsätter hello åtgärden resulterar i ett tillfälligt avbrott i hello-tjänsten och du uppmanas att bekräfta.
* **Stänga av en domänkontrollant för en aktiv** – du meddelas att fortsätter hello åtgärden resulterar i driftstopp. Du måste också toopush hello strömknappen på en eller båda domänkontrollanterna tooturn på hello enhet. Du uppmanas att bekräfta.

**F.** När hello controller omstart eller avstängning misslyckas tooprogress?

**S.** Starta om eller stänga av en domänkontrollant kan misslyckas om:

* En enhet uppdatering pågår.
* En omstart av domänkontrollanten pågår redan.
* En domänkontrollant avstängning pågår redan.

**F.** Hur kan du ta reda på om en domänkontrollant har startats om eller stänga av?

**S.** Du kan kontrollera statusen för hello-domänkontrollant på domänkontrollanten bladet. status för hello domänkontrollanten indikerar om en domänkontrollant är hello processen för att starta om eller stänga av. Dessutom hello **aviseringar** bladet innehålla en informationsavisering om hello controller startas om eller stänga av. hello controller omstart och avstängning operations registreras också i hello acitivity loggar. Mer information om acitivity loggar gå för[visa hello aktivitetsloggar](storsimple-8000-service-dashboard.md#view-the-activity-logs).

**F.** Finns det någon inverkan toohello i/o på grund av domänkontrollant växling vid fel?

**S.** hello TCP-anslutningar mellan initierare och aktiva styrenheten kommer att återställas till följd av domänkontrollant växling vid fel, men återupprättas när hello passiva controller förutsätter igen. Det kan vara en tillfällig (mindre än 30 sekunder)-paus i i/o-aktivitet mellan initierare och hello enhet under hello loppet av den här åtgärden.

**F.** Hur återställer jag min controller tooservice när det stängs av och tas bort?

**S.** tooreturn tooservice en domänkontrollant måste du infoga den i hello chassi som beskrivs i [ersätta en domänkontrollant modul på din StorSimple-enhet](storsimple-8000-controller-replacement.md).

## <a name="next-steps"></a>Nästa steg
* Om det uppstår problem med din StorSimple-styrenheter som du inte kan lösa med hjälp av hello procedurer som anges i den här självstudien [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* toolearn mer information om hur du använder hello StorSimple Enhetshanteraren tjänst, gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

