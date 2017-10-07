---
title: aaaManage StorSimple-styrenheter | Microsoft Docs
description: "Lär dig hur toostop, starta om, stänga av eller återställa din StorSimple-styrenheter."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4ee989d0-956f-4c14-951e-fd4e490ea09d
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2016
ms.author: alkohli
ms.openlocfilehash: 9a86aa0f4a8fd96c36df206774972602c47a49a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Hantera din StorSimple-styrenheter
## <a name="overview"></a>Översikt
Den här självstudiekursen beskriver hello olika åtgärder som kan utföras på din StorSimple-styrenheter. hello-styrenheter i din StorSimple-enhet är redundant (peer) domänkontrollanter i ett aktivt-passivt konfiguration. Endast en domänkontrollant är aktiv vid en given tidpunkt och bearbetar alla hello disk- och åtgärder. hello är andra domänkontrollanter i passivt läge. Om hello aktiva styrenheten misslyckas aktiveras hello passiva controller automatiskt.

Vägledningen innehåller stegvisa instruktioner toomanage hello-styrenheter med hjälp av den:

* **Domänkontrollanter** avsnitt i hello **Underhåll** sida i hello StorSimple Manager-tjänsten
* Windows PowerShell för StorSimple.

Vi rekommenderar att du hanterar hello styrenheter via hello StorSimple Manager-tjänsten. Om en åtgärd kan endast utföras med hjälp av Windows PowerShell för StorSimple, gör en del av hello kursen.

När du har läst den här självstudiekursen kommer du att kunna:

* Starta om eller stänga av en domänkontrollant för StorSimple-enhet
* Stänga av en StorSimple-enhet
* Återställ standardvärden för toofactory din StorSimple-enhet

## <a name="restart-or-shut-down-a-single-controller"></a>Starta om eller stänga av en enda domänkontrollant
En domänkontrollant omstart eller avstängning krävs inte som en del av normal drift. Avstängning åtgärder för en enskild enhet är vanliga endast i fall där en misslyckad enhet maskinvarukomponent behöver bytas ut. En omstart av domänkontrollanten kan också krävas i en situation där prestanda påverkas av omfattande minnesanvändning eller en felaktig styrenhet. Du kanske också måste toorestart en domänkontrollant efter en lyckad controller ersättning, om du vill tooenable och testa hello ersättas domänkontrollant.

Starta om en enhet är inte störande tooconnected initierare, förutsatt att hello passiva domänkontrollant är tillgänglig. Om en passiv domänkontrollant är inte tillgänglig eller inaktiverad, startar om hello active kan styrenhet resultera i hello avbrott i tjänsten och driftstopp.

> [!IMPORTANT]
> * **En domänkontrollant som körs ska aldrig tas fysiskt bort eftersom det skulle resultera i förlust av redundans och en ökad risk för driftstopp.**
> * hello gäller följande procedur bara toohello fysiska StorSimple-enheten. Information om hur toostart, stoppa och starta om hello virtuell enhet, se [arbeta med hello virtuella enheten](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).
> 
> 

Du kan starta om eller stänga av en enskild enhet domänkontrollant med hjälp av hello klassiska Azure-portalen för hello StorSimple Manager-tjänsten eller Windows PowerShell för StorSimple

toomanage din styrenheter från hello klassiska Azure-portalen utföra hello följande steg.

#### <a name="toorestart-or-shut-down-a-controller-in-classic-portal"></a>toorestart eller Stäng av en domänkontrollant i den klassiska portalen
1. Navigera för**enheter > Underhåll**.
2. Gå för**maskinvarustatus** och kontrollera att hello status för båda hello domänkontrollanter på din enhet är **felfri**.
   
    ![Kontrollera StorSimple-styrenheter är felfria](./media/storsimple-manage-device-controller/IC766017.png)
3. Hello längst ned i hello **Underhåll** klickar du på **hantera domänkontrollanter**.
   
    ![Hantera StorSimple-styrenheter](./media/storsimple-manage-device-controller/IC766018.png)</br>
   
   > [!NOTE]
   > Om du inte ser **hantera domänkontrollanter**, måste du tooinstall uppdateringar. Mer information finns i [uppdatera din StorSimple-enhet](storsimple-update-device.md).
   > 
   > 
4. I hello **ändra inställningarna för domänkontrollanter** dialogrutan rutan, hello följande:
   
   1. Från hello **Select Controller** listrutan, Välj hello domänkontrollant som du vill toomanage. hello alternativ är styrenhet 0 och 1. Dessa domänkontrollanter identifieras också aktiva eller passiva.
      
      > [!NOTE]
      > En domänkontrollant inte kan hanteras om det är inte tillgänglig eller aktiverade inaktiverat och visas inte i hello listrutan.
      > 
      > 
   2. Från hello **Välj åtgärd** listrutan väljer du **omstart controller** eller **Stäng controller**.
      
       ![Starta om passiva domänkontrollanten för StorSimple-enhet](./media/storsimple-manage-device-controller/IC766020.png)
   3. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-device-controller/IC740895.png).

Detta kommer att starta om eller stänga hello-styrenhet. hello tabellen nedan sammanfattar hello information om vad som händer beroende på hello val du gjorde i hello **ändra inställningarna för domänkontrollanter** dialogrutan.  

| Val av # | Om du vill... | Detta sker. |
| --- | --- | --- |
| 1. |Starta om hello passiva domänkontrollanten. |Ett jobb skapas toorestart hello domänkontrollant och du meddelas när hello jobbet har skapats. Detta initierar hello controller startas om. Du kan övervaka hello omstarten genom att öppna **Service > instrumentpanelen > Visa åtgärdsloggar** och sedan filtrering av parametrar specifika tooyour-tjänsten. |
| 2. |Starta om hello aktiva styrenhet. |Du ser hello följande varning: ”Om du startar om hello aktiva styrenheten hello enhet misslyckas över toohello passiva domänkontrollant. Vill du toocontinue ”? </br>Om du väljer tooproceed med den här åtgärden hello nästa steg är identiska toothose används toorestart hello passiva domänkontrollant (se markeringen 1). |
| 3. |Stäng hello passiva domänkontrollant. |Hello följande meddelande visas ”: när har avslutats, måste toopush hello power knappen på din domänkontrollant tooturn den på. Vill du verkligen tooshut ned den här styrenheten ”? </br>Om du väljer tooproceed med den här åtgärden hello nästa steg är identiska toothose används toorestart hello passiva domänkontrollant (se markeringen 1). |
| 4. |Stäng hello aktiva styrenhet. |Hello följande meddelande visas ”: när har avslutats, måste toopush hello power knappen på din domänkontrollant tooturn den på. Vill du verkligen tooshut ned den här styrenheten ”? </br>Om du väljer tooproceed med den här åtgärden hello nästa steg är identiska toothose används toorestart hello passiva domänkontrollant (se markeringen 1). |

#### <a name="toorestart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>toorestart eller Stäng av en domänkontrollant i Windows PowerShell för StorSimple
Utför följande steg tooshut ned hello eller starta om en enda domänkontrollanten på StorSimple-enheten från hello klassiska Azure-portalen.

1. Hello enhet med hjälp av hello seriekonsolen eller en telnet-session från en fjärrdator. Anslut tooController 0 eller domänkontrollant 1 av följande hello stegen i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.
3. I hello Banderollmeddelandet anteckna hello-domänkontrollant som du är ansluten för (styrenhet 0 eller 1-styrenhet) och om det är hello aktiva eller passiva (standby) hello-styrenheten.
   
   * tooshut ned en enda domänkontrollant i Kommandotolken hello typ:
     
       `Stop-HcsController`
     
       Detta stängs hello-domänkontrollant som du är ansluten till. Om du stoppar hello aktiva styrenheten, misslyckas det över toohello passiva domänkontrollant innan den stängs av.
   * toorestart skriver du en domänkontrollant i Kommandotolken hello:
     
       `Restart-HcsController`
     
       Hello-domänkontrollant som du är ansluten till kommer att startas om. Om du startar om hello aktiva styrenheten, växlar den över toohello passiva domänkontrollant innan hello startar om.

## <a name="shut-down-a-storsimple-device"></a>Stänga av en StorSimple-enhet
Det här avsnittet beskrivs hur tooshut ned en löpande eller en misslyckad StorSimple-enhet från en fjärrdator. En enhet är inaktiverat när du stänger av både hello-styrenheter. En enhet avstängning görs när hello enhet fysiskt flyttas eller tas ur funktion.

> [!IMPORTANT]
> Kontrollera hello hälsa hello komponenter innan du stänger hello enhet. Navigera för**enheter > Underhåll > maskinvarustatus** och kontrollera att hello Indikator status för alla hello-komponenter är grön. Endast en felfri enhet har en grön status. Om enheten är att stänga av tooreplace en felaktig komponent, visas en misslyckad (röd) eller en försämrad (gul) status för hello respektive komponenterna.
> 
> 

#### <a name="tooshut-down-a-storsimple-device"></a>tooshut ned en StorSimple-enhet
1. Använd hello [starta om eller stänga av en domänkontrollant](#restart-or-shut-down-a-single-controller) proceduren tooidentify och avsluta hello passiva domänkontrollant på enheten. Du kan utföra den här åtgärden i hello klassiska Azure-portalen eller i Windows PowerShell för StorSimple.
2. Upprepa hello senare steg tooshut ned hello aktiva styrenhet.
3. Nu behöver du toolook på hello tillbaka plana av hello enhet. När hello två domänkontrollanter helt är stängt ska hello status indikatorer på båda hello domänkontrollanter blinkande red. Om du behöver tooturn av hello enheten helt just nu Vänd hello power växlar på både ström och kylning moduler (PCMs) toohello OFF-läge. Detta ska stänga av hello enhet.

<!--#### tooshut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect toohello serial console of hello StorSimple device by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In hello serial console menu, verify from hello banner message that hello controller you are connected toois hello passive controller. If you are connected toohello active controller, disconnect from this controller and connect toohello other controller.

1. In hello serial console menu, choose option 1, **log in with full access**.

1. At hello prompt, type:

    `Stop-HCSController`

    This should shut down hello current controller. tooverify whether hello shutdown has finished, check hello back of hello device. hello controller status LED should be solid red.

1. Repeat steps 1 through 4 tooconnect toohello active controller and then shut it down.

1. After both hello controllers are completely shut down, hello status LEDs on both should be blinking red. If you need tooturn off hello device completely at this time, flip hello power switches on both Power and Cooling Modules (PCMs) toohello OFF position.-->

## <a name="reset-hello-device-toofactory-default-settings"></a>Återställ standardinställningar för hello enheten toofactory
> [!IMPORTANT]
> Kontakta Microsoft Support om du behöver tooreset toofactory standardinställningarna på enheten. hello proceduren som beskrivs nedan bör endast användas tillsammans med Microsoft-supporten.
> 
> 

Den här proceduren beskriver hur tooreset din Microsoft Azure StorSimple enheten toofactory standardinställningar med hjälp av Windows PowerShell för StorSimple.
När du återställer en enhet tar bort alla data och inställningar från hello hela klustret som standard.

Utför följande steg tooreset hello standardinställningarna för Microsoft Azure StorSimple-enhet toofactory:

### <a name="tooreset-hello-device-toodefault-settings-in-windows-powershell-for-storsimple"></a>tooreset hello toodefault Enhetsinställningar i Windows PowerShell för StorSimple
1. Hello enhet via dess seriekonsol. Kontrollera hello banderoll meddelandet tooensure att du är ansluten toohello aktiva styrenhet.
2. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.
3. I hello kommandotolk, skriver du hello efter kommandot tooreset hello hela klustret, ta bort alla data och metadata för domänkontrollanten inställningar:
   
    `Reset-HcsFactoryDefault`
   
    tooinstead återställa en enda domänkontrollant använder hello [Återställ HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) med hello `-scope` parameter.)
   
    hello systemet startas om flera gånger. Du meddelas när hello återställning har slutförts. Det kan ta den här processen 45 – 60 minuter under en 8100-enhet och 60-90 minuter för en 8600 toofinish beroende på hello systemmodell.
   
   > [!TIP]
   > * Om du använder Update 1.2 eller tidigare använda hello `–SkipFirmwareVersionCheck` versionskontroll för parametern tooskip hello firmware (annars ser du en firmware-matchningsfel: fabriksåterställning kan inte fortsätta på grund av tooa matchning av datatyp i versioner av inbyggd hello. ).
   > * hello kan factory återställning fungera för StorSimple-enheter som kör uppdatering 1 eller 1.1 i hello Government portal och har utfört en lyckad enkel eller dubbel domänkontrollant ersättning (med ersättning domänkontrollanter som levererades med före uppdatering 1 programvara). Detta händer när hello fabriksåterställa avbildningen verifieras för hello förekomsten av en SHA1-fil på hello-domänkontrollant som inte finns för före uppdateringen 1 programvara. Om du ser den här fabriken återställa fel, kontakta Microsoft Support tooassist steg du med hello nästa. Det här problemet visas inte med ersättning domänkontrollanter som levererades från hello factory med uppdatering 1 eller senare programvara.
   > 
   > 

## <a name="questions-and-answers-about-managing-device-controllers"></a>Frågor och svar om hur du hanterar styrenheter
I det här avsnittet har vi sammanfattas några av hello vanliga frågor angående hantera StorSimple-styrenheter.

**F.** Vad händer om båda hello domänkontrollanter på enheten är felfri och aktiverade och starta om eller stänga hello aktiva styrenheten?

**S.** Om båda hello domänkontrollanter på din enhet är felfri och aktiverade på, du uppmanas att bekräfta. Du kan välja att:

* **Starta om hello aktiva styrenheten** – du uppmanas att starta om en domänkontrollant för active resulterar hello enheten toofail över toohello passiva domänkontrollant. hello domänkontrollanten startar om.
* **Stänga av en domänkontrollant för en aktiv** – du meddelas att stänga av en domänkontrollant för active leder till avbrott. Du måste också toopush hello strömknappen på hello enheten tooturn på hello-domänkontrollant.

**F.** Vad händer om hello passiva domänkontrollant på enheten är inte tillgänglig eller aktiverade inaktiverat och jag startar om eller stänger hello aktiva styrenheten?

**S.** Om hello passiva domänkontrollanten på din enhet är tillgänglig eller så är aktiverade inaktiverat och du väljer att:

* **Starta om hello aktiva styrenheten** – du meddelas att fortsätter hello åtgärden resulterar i ett tillfälligt avbrott i hello-tjänsten och du uppmanas att bekräfta.
* **Stänga av en domänkontrollant för en aktiv** – du meddelas att fortsätter hello åtgärden resulterar i avbrottstid och att du måste toopush hello strömknappen på en eller båda domänkontrollanterna tooturn på hello enhet. Du uppmanas att bekräfta.

**F.** När hello controller omstart eller avstängning misslyckas tooprogress?

**S.** Starta om eller stänga av en domänkontrollant kan misslyckas om:

* En enhet uppdatering pågår.
* En omstart av domänkontrollanten pågår redan.
* En domänkontrollant avstängning pågår redan.

**F.** Hur kan du ta reda på om en domänkontrollant har startats om eller stänga av?

**S.** Du kan kontrollera status för hello domänkontrollanten på sidan för hello underhåll. status för hello domänkontrollanten indikerar om en domänkontrollant har startats om eller stänga av. Dessutom innehåller hello aviseringar sidan en informationsavisering om hello domänkontrollant har startats om eller stänga av. hello controller omstart och avstängning operations registreras också i hello åtgärdsloggar. Mer information om åtgärdsloggar gå för[visa hello åtgärdsloggar](storsimple-service-dashboard.md#view-the-operations-logs).

**F.** Finns det någon effekt toohello I/o på grund av domänkontrollant växling vid fel?

**S.** hello TCP-anslutningar mellan initierare och aktiva styrenheten kommer att återställas till följd av domänkontrollant växling vid fel, men återupprättas när hello passiva controller förutsätter igen. Det kan vara en tillfällig (mindre än 30 sekunder)-paus i i/o-aktivitet mellan initierare och hello enhet under hello loppet av den här åtgärden.

**F.** Hur återställer jag min controller tooservice när det stängs av och tas bort?

**S.** tooreturn tooservice en domänkontrollant måste du infoga den i hello chassi som beskrivs i [ersätta en domänkontrollant modul på din StorSimple-enhet](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Nästa steg
* Om det uppstår problem med din StorSimple-styrenheter som du inte kan lösa med hjälp av hello procedurer som anges i den här självstudien [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).
* Gå toolearn mer information om hur du använder hello StorSimple Manager-tjänsten för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

