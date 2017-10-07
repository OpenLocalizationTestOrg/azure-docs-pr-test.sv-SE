---
title: aaaView och hantera Microsoft Azure StorSimple virtuell matris aviseringar | Microsoft Docs
description: "Beskriver virtuella StorSimple-matris avisering villkor och allvarlighetsgrad och hur toouse hello StorSimple Manager-tjänsten toomanage aviseringar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0fb5b1b9064f33df1d8fa7ace45f0d72b0a1622
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-alerts-for-hello-storsimple-virtual-array"></a>Använd StorSimple Enhetshanteraren toomanage aviseringar för hello virtuella StorSimple-matris

## <a name="overview"></a>Översikt

hello aviseringar funktion i hello StorSimple enheten Manager-tjänsten gör det möjligt för du tooreview och avmarkera aviseringar relaterade tooStorSimple virtuella matriser på ett i realtid. Du kan använda hello aviseringar på hello **sammanfattning av tjänst** bladet toocentrally övervaka hello problem för din virtuella StorSimple-matriser och hello övergripande Microsoft Azure StorSimple-lösningen.

Den här självstudiekursen beskriver hur tooconfigure aviseringar, vanliga avisering villkor varningsnivåer allvarlighetsgrad och hur tooview och spåra aviseringar. Dessutom innehåller aviseringen Snabbreferens tabeller, vilket gör du tooquickly Leta upp en specifik avisering och svara på lämpligt sätt.

![Sidan varningar](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Konfigurera aviseringsinställningar

Du kan välja om du vill toobe meddelas via e-post med hello avisering villkor för var och en av din virtuella StorSimple-matriser. Dessutom kan du identifiera andra mottagare varningsmeddelanden genom att ange sina e-postadresser i hello **ytterligare e-postmottagare** rutan, avgränsade med semikolon.

> [!NOTE]
> Du kan ange högst 20 e-postadresser per virtuell matris.


När du aktiverar e-postmeddelanden för en virtuell matris får medlemmar i listan över hello-meddelande ett e-postmeddelande varje gång en kritisk varning inträffar. hello-meddelanden skickas från  *storsimple-alerts-noreply@mail.windowsazure.com*  och beskriver hello varningsvillkor. Mottagarna kan klicka på **Unsubscribe** tooremove själva hello e-postavisering listan.

#### <a name="tooenable-email-notification-for-alerts"></a>tooenable e-postmeddelande för aviseringar

1. Gå tooyour StorSimple Enhetshanteraren tjänsten och i hello **Management** markerar och klicka på **enheter**. Välj hello lista över enheter som visas och klicka på enheten.
   
    ![aviseringsinställningar](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Detta öppnar hello **inställningar** bladet. I hello **Enhetsinställningar** väljer **allmänna**. Detta öppnar hello **allmänna inställningar** bladet.
   
    ![Aviseringskonfiguration för aviseringar](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. I hello **allmänna inställningar** bladet går för**varningsinställningar** och ange hello följande:
   
   1. I hello **Aktivera e-postmeddelanden** väljer **Ja**.
   2. I hello **e-tjänstadministratörer** väljer **Ja** om du vill toohave hello tjänstadministratören och medadministratörer för alla får hello varningsmeddelanden.
   3. I hello **ytterligare e-postmottagare** anger hello e-postadresserna för alla andra mottagare som får hello varningsmeddelanden. Skriv namnen i formatet hello  *someone@somewhere.com* . Använd semikolon tooseparate hello e-postadresser. Du kan konfigurera högst 20 e-postadresser per virtuell enhet.
      
       ![Aviseringskonfiguration för aviseringar](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. toosend test e-postaviseringar, klickar du på **skicka testmeddelandet**. Hej StorSimple enheten Manager-tjänsten visar statusmeddelanden som vidarebefordrar hello testmeddelande.
      
       ![Aviseringar testa e-postmeddelande skickas](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Om hello testar meddelandet kan inte skickas, hello StorSimple Enhetshanteraren tjänsten visas ett meddelande. Klicka på **OK**Vänta några minuter och försök sedan toosend din anmälan testmeddelande igen.
      > 
      > 
   5. Hello längst hello-sidan, klickar du på **spara** toosave din konfiguration. Klicka på **Ja** när du uppmanas att bekräfta åtgärden.
      
      ![Aviseringar testa e-postmeddelande skickas](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Vanliga avisering villkor

Din virtuella StorSimple-matris genererar varningar i svaret tooa olika villkor. hello följande är hello de vanligaste typerna av aviseringen villkor:

* **Problem med nätverksanslutningen** – dessa aviseringar inträffar när det är svårt att överföra data. Kommunikationsproblem kan uppstå under överföring av data tooand från hello Azure storage-konto eller förfallodatum toolack för anslutningen mellan hello virtuella enheter och hello StorSimple Device Manager-tjänsten. Kommunikationsproblem är några av hello svåraste toofix eftersom det finns så många felpunkter. Du bör alltid först kontrollera att nätverksanslutningen och Internet-åtkomst är tillgängliga innan du fortsätter toomore avancerad felsökning. Information om portar och brandväggsinställningar finns för[virtuella StorSimple-matris systemkrav](storsimple-ova-system-requirements.md). För hjälp med felsökning kan du gå för[felsökning med hello Test-Connection cmdlet](storsimple-troubleshoot-deployment.md).
* **Prestandaproblem** – aviseringarna orsakas när systemet inte är presterar optimalt, till exempel när det är hårt belastat.

Du kan dessutom se aviseringar relaterade toosecurity, uppdateringar eller jobbfel.

## <a name="alert-severity-levels"></a>Nivåer för aviseringarnas allvarlighetsgrad

Aviseringar har olika allvarlighetsgrader, beroende på hello inverkan som hello kommer aviseringen situationen har och hello behovet av att en avisering om toohello svar. hello allvarlighetsgrader finns:

* **Kritiska** – den här aviseringen är i svaret tooa villkor som påverkar hello lyckade prestanda. Åtgärden är obligatorisk tooensure hello StorSimple-tjänsten inte avbryts.
* **Varning** – det här tillståndet kan bli kritiska om inte matcha. Du bör undersöka hello situation och vidta någon åtgärd krävs tooclear hello problemet.
* **Information** – den här aviseringen innehåller information som kan vara användbar vid spåra och hantera datorn.

## <a name="view-and-track-alerts"></a>Visa och spåra aviseringar

Hej StorSimple Enhetshanteraren service sammanfattning bladet ger dig en snabb glimt hello antalet aviseringar på dina virtuella enheter, ordnade efter allvarlighetsgrad.

![Instrumentpanel för aviseringar](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Klicka på hello allvarlighetsgrad nivå tooopen hello **aviseringar** bladet. hello resultat är endast hello-aviseringar som matchar den allvarlighetsgraden.

![Aviseringsrapporten omfång tooalert typ](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Klicka på en avisering i hello tooget ytterligare information för hello aviseringen, inklusive hello tid för senaste hello avisering rapporterades hello antalet förekomster av hello aviseringen på hello enhet och hello rekommenderad åtgärd tooresolve hello avisering.

![Aviseringslistan och information](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Om du behöver toosend hello information tooMicrosoft Support kan du kopiera hello aviseringsinformation tooa textfil. När du har följt hello rekommendation hello varningsvillkor lokalt ska du avmarkera hello avisering hello listan. Välj hello avisering hello listan och klicka sedan på **Rensa**. tooclear flera aviseringar, väljer varje varning, klicka på alla kolumner utom hello **avisering** kolumnen och klicka sedan på **Rensa** när du har valt alla hello aviseringar toobe avmarkerad.

När du klickar på **Rensa**, måste hello möjlighet tooprovide kommentarer om hello aviseringen och hello steg som du tog tooresolve hello problemet. 

![aviseringen kommentarer](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Vissa händelser rensas hello systemet om en annan händelse utlöses med ny information. 

## <a name="sort-and-review-alerts"></a>Sortera och granska aviseringar

Hej **aviseringar** bladet kan visa in too250 aviseringar. Om du har överskridit det antalet aviseringar visas inte alla aviseringar i hello standardvyn. Du kan kombinera hello följande fält toocustomize aviseringar visas:

* **Status för** – du kan visa antingen **Active** eller **avmarkerad** aviseringar. Aktiva aviseringar aktiveras fortfarande i systemet, medan avmarkerad aviseringar antingen har rensats manuellt av en administratör eller programmatiskt eftersom hello system uppdatera hello varningsvillkor med ny information.
* **Allvarlighetsgrad** – du kan visa aviseringar för alla allvarlighetsgrader (kritisk, varning, information) eller bara en viss allvarlighetsgrad, till exempel endast kritiska aviseringar.
* **Källan** – du kan visa aviseringar från alla källor eller begränsa hello aviseringar toothose som kommer från hello service eller en eller alla hello virtuella enheter.
* **Tidsintervallet** – genom att ange hello **från** och **till** datum och tidsstämplar du tittar på aviseringar under hello tidsperiod som du är intresserad av.

## <a name="alerts-quick-reference"></a>Snabbreferens för aviseringar

hello följande tabeller listar några av hello StorSimple aviseringar som kan uppstå, samt ytterligare information och rekommendationer i förekommande fall. StorSimple virtuell matris aviseringar indelas i följande kategorier hello:

* [Molnet anslutningsvarningar](#cloud-connectivity-alerts)
* [Konfigurationsaviseringar](#configuration-alerts)
* [Jobbet aviseringar](#job-failure-alerts)
* [Prestandavarningar](#performance-alerts)
* [Säkerhetsaviseringar](#security-alerts)
* [Uppdatera aviseringar](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Molnet anslutningsvarningar

| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Enheten  *<device name>*  är inte ansluten toohello moln. |hello med namnet enheten kan inte ansluta toohello moln. |Kunde inte ansluta toohello moln. Detta kan bero på grund av tooone av hello följande:<ul><li>Det kan finnas problem med hello nätverksinställningar på enheten.</li><li>Det kan finnas problem med hello lagringskontouppgifter.</li></ul>Mer information om hur du felsöker problem med nätverksanslutningen finns toohello [lokala webbgränssnittet](storsimple-ova-web-ui-admin.md) av hello enhet. |

### <a name="configuration-alerts"></a>Konfigurationsaviseringar

| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Lokala virtuella enhetskonfigurationen stöds inte. |Långsam prestanda. |hello aktuell konfiguration kan resultera i försämrade prestanda. Kontrollera att servern uppfyller hello lägsta konfigurationskrav. Mer information finns för[krav för virtuell StorSimple-matris](storsimple-ova-system-requirements.md). |
| Du kör allokerade diskutrymmet är slut på <*enhetsnamn*>. |Disk varning. |Du saknar etablerade diskutrymme. toofree utrymme, Överväg att flytta arbetsbelastningar tooanother volym eller resurs eller ta bort data. |

### <a name="job-failure-alerts"></a>Jobbet aviseringar

| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Säkerhetskopiering av <*enhetsnamn*> Det gick inte att slutföra. |Säkerhetskopieringsjobbet misslyckades. |Det gick inte att skapa en säkerhetskopia. Använd någon av följande hello:<ul><li>Problem med nätverksanslutningen kan förhindra hello säkerhetskopieringen från en lyckad. Se till att det inte finns några anslutningsproblem. Mer information om hur du felsöker problem med nätverksanslutningen finns toohello [lokala webbgränssnittet](storsimple-ova-web-ui-admin.md) för din virtuella enhet.</li><li>Du har nått gränsen för hello tillgängligt lagringsutrymme. toofree utrymme, bör du ta bort alla säkerhetskopieringar som inte längre behövs.</li></ul> Lös problemen hello, avmarkera hello aviseringen och försök hello igen. |
| Klona av <*enhetsnamn*> Det gick inte att slutföra. |Klona jobbet misslyckades. |Det gick inte att skapa en klon. Använd någon av följande hello:<ul><li>Säkerhetskopiera listan kanske inte giltig. Uppdatera hello listan tooverify den fortfarande är giltig.</li><li>Problem med nätverksanslutningen kan förhindra hello kopieringen från en lyckad. Se till att det inte finns några anslutningsproblem.</li><li>Du har nått gränsen för hello tillgängligt lagringsutrymme. toofree utrymme, bör du ta bort alla säkerhetskopieringar som inte längre behövs.</li></ul>Lös problemen hello, avmarkera hello aviseringen och försök hello igen. |

### <a name="performance-alerts"></a>Prestandavarningar

| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Du fördröjs oväntat dataöverföring. |Långsam dataöverföring. |Bandbreddsbegränsning fel uppstår när du överskrider hello skalbarhetsmål för storage-tjänst. hello storage-tjänst har den här tooensure som ingen enskild klient eller innehavare kan använda hello service på hello bekostnad av andra. Mer information om hur du felsöker ditt Azure storage-konto finns för[övervaka, diagnostisera och felsöka Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| Du har ont om lokala reservation ledigt diskutrymme på <*enhetsnamn*>. |Långsam svarstid. |10% av hello total etablerade storlek för <*enhetsnamn*> är reserverad på hello lokala enhet och du är nu ont om hello reserverat minne. hello arbetsbelastningen på <*enhetsnamn*> genererar ett större antal omsättning eller du kanske har nyligen har migreras stora mängder data. Detta kan medföra nedsatt prestanda. Använd någon av följande åtgärder tooresolve hello detta:<ul><li>Öka hello moln bandbredd toothis enhet.</li><li>Minska eller flytta arbetsbelastningar tooanother volym eller resurs.</li></ul> |

### <a name="security-alerts"></a>Säkerhetsaviseringar

| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Lösenordet för <*enhetsnamn*> upphör att gälla om <*nummer*> dagar. |Varning för lösenord. |Ditt lösenord upphör att gälla om < nummer < dagar. Överväg att ändra ditt lösenord. Mer information finns för[ändra hello virtuella StorSimple-matris enhetens administratörslösenord](storsimple-virtual-array-change-device-admin-password.md). |

### <a name="update-alerts"></a>Uppdatera aviseringar

| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Det finns nya uppdateringar för din enhet. |Det finns uppdateringar toohello virtuella StorSimple-matris. |Du kan installera nya uppdateringar från hello **Underhåll** sidan. |
| Kunde inte söka efter nya uppdateringar på <*enhetsnamn*>. |Uppdatera fel. |Ett fel uppstod när du installerar nya uppdateringar. Du kan manuellt installera hello uppdateringar. Om hello problemet kvarstår kontaktar du [Microsoft-supporten](storsimple-contact-microsoft-support.md). |

## <a name="next-steps"></a>Nästa steg

* [Lär dig mer om hello virtuella StorSimple-matris](storsimple-ova-overview.md).

