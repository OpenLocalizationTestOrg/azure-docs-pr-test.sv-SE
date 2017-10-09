---
title: "aaaView och hantera aviseringar för StorSimple | Microsoft Docs"
description: "Beskriver StorSimple avisering villkor och allvarlighetsgrad, hur tooconfigure aviseringar och hur toouse hello StorSimple Manager-tjänsten toomanage aviseringar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bee49253-9ac7-4131-95f6-6bf0e72b8438
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/08/2017
ms.author: anbacker
ms.openlocfilehash: d322c88b565606538a3acb61ff939ec1fbe2f3cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-alerts"></a>Använd hello StorSimple Manager-tjänsten tooview och hantera aviseringar för StorSimple
## <a name="overview"></a>Översikt
Hej **aviseringar** fliken i hello StorSimple Manager-tjänsten kan du tooreview och avmarkera StorSimple-enhet – relaterade aviseringar på en i realtid. Från den här fliken kan du centralt övervaka hello problem för din StorSimple-enheter och hello övergripande Microsoft Azure StorSimple-lösningen.

Den här självstudiekursen beskriver vanliga avisering villkor, allvarlighetsgrad nivåer och hur tooconfigure aviseringar. Dessutom innehåller aviseringen Snabbreferens tabeller, vilket gör du tooquickly Leta upp en specifik avisering och svara på lämpligt sätt.

![Sidan varningar](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Vanliga avisering villkor
StorSimple-enheten genererar varningar i svaret tooa olika villkor. hello följande är hello de vanligaste typerna av aviseringen villkor:

* **Problem med maskinvara** – dessa aviseringar meddelar dig om hello hälsotillstånd maskinvara. De kan du vet om firmware-uppgraderingar krävs om ett nätverksgränssnitt har problem, eller om det är problem med en av dina enheter.
* **Problem med nätverksanslutningen** – dessa aviseringar inträffar när det är svårt att överföra data. Kommunikationsproblem kan uppstå under överföring av data tooand från hello Azure storage-konto eller förfallodatum toolack för anslutningen mellan hello enheter och hello StorSimple Manager-tjänsten. Kommunikationsproblem är några av hello svåraste toofix eftersom det finns så många felpunkter. Du bör alltid först kontrollera att nätverksanslutningen och Internet-åtkomst är tillgängliga innan du fortsätter toomore avancerad felsökning. För hjälp med felsökning kan du gå för[felsökning med hello Test-Connection cmdlet](storsimple-troubleshoot-deployment.md).
* **Prestandaproblem** – aviseringarna orsakas när systemet inte är presterar optimalt, till exempel när det är hårt belastat.

Du kan dessutom se aviseringar relaterade toosecurity, uppdateringar eller jobbfel.

## <a name="alert-severity-levels"></a>Nivåer för aviseringarnas allvarlighetsgrad
Aviseringar har olika allvarlighetsgrader, beroende på hello inverkan som hello kommer aviseringen situationen har och hello behovet av att en avisering om toohello svar. hello allvarlighetsgrader finns:

* **Kritiska** – den här aviseringen är i svaret tooa villkor som påverkar hello lyckade prestanda. Åtgärden är obligatorisk tooensure hello StorSimple-tjänsten inte avbryts.
* **Varning** – det här tillståndet kan bli kritiska om inte matcha. Du bör undersöka hello situation och vidta någon åtgärd krävs tooclear hello problemet.
* **Information** – den här aviseringen innehåller information som kan vara användbar vid spåra och hantera datorn.

## <a name="configure-alert-settings"></a>Konfigurera aviseringsinställningar
Du kan välja om du vill toobe meddelas via e-post med aviseringen villkor för var och en av dina StorSimple-enheter. Dessutom kan du identifiera andra mottagare varningsmeddelanden genom att ange sina e-postadresser i hello **andra e-postmottagare** rutan, avgränsade med semikolon.

> [!NOTE]
> Du kan ange högst 20 e-postadresser per enhet.
> 
> 

När du aktiverar e-postmeddelanden för en enhet kan får medlemmar i listan över hello-meddelande ett e-postmeddelande varje gång en kritisk varning inträffar. hello-meddelanden skickas från  *storsimple-alerts-noreply@mail.windowsazure.com*  och beskriver hello varningsvillkor. Mottagarna kan klicka på **Unsubscribe** tooremove själva hello e-postavisering listan.

#### <a name="tooenable-email-notification-of-alerts-for-a-device"></a>tooenable e-postmeddelanden om aviseringar för en enhet
1. Gå för**enheter** > **konfigurera** för hello enhet.
2. Under **aviseringsinställningar**, ange hello följande:
   
   1. I hello **skicka e-postavisering** väljer **Ja**.
   2. I hello **e-tjänstadministratörer** väljer **Ja** om du vill toohave hello tjänstadministratören och medadministratörer för alla får hello varningsmeddelanden.
   3. I hello **andra e-postmottagare** anger hello e-postadresserna för alla andra mottagare som får hello varningsmeddelanden. Skriv namnen i formatet hello  *someone@somewhere.com* . Använd semikolon tooseparate hello e-postadresser. Du kan konfigurera högst 20 e-postadresser per enhet. 
      
       ![Aviseringskonfiguration för aviseringar](./media/storsimple-manage-alerts/AlertNotify.png)
3. toosend test e-postaviseringar, klicka på pilikonen hello nästa för**skicka testmeddelandet**. Hej StorSimple Manager-tjänsten visar statusmeddelanden som vidarebefordrar hello testmeddelande. 
4. När hello följande meddelande visas, klickar du på **OK**. 
   
    ![Aviseringar testa e-postmeddelande skickas](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)
   
   > [!NOTE]
   > Om inte hello testmeddelandet kan skickas, visas ett meddelande med hello StorSimple Manager-tjänsten. Klicka på **OK**Vänta några minuter och försök sedan toosend din anmälan testmeddelande igen. 
   > 
   > 

## <a name="view-and-track-alerts"></a>Visa och spåra aviseringar
Hej StorSimple Manager-tjänsten instrumentpanelen ger dig en snabb glimt hello antalet aviseringar på dina enheter, ordnade efter allvarlighetsgrad.

![Instrumentpanel för aviseringar](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Om du klickar på hello allvarlighetsgrad öppnas hello **aviseringar** fliken hello resultat är endast hello-aviseringar som matchar den allvarlighetsgraden.

![Aviseringsrapporten omfång tooalert typ](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Att klicka på en avisering i hello lista innehåller ytterligare information för hello avisering, inklusive hello senaste gången hello varning rapporterades, hello antalet förekomster av hello avisering på hello enhet och hello rekommenderad åtgärd tooresolve hello avisering. Om det är en avisering om maskinvara, kommer det också identifiera hello maskinvarukomponent.

![Maskinvara avisering exempel](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Om du behöver toosend hello information tooMicrosoft Support kan du kopiera hello aviseringsinformation tooa textfil. När du har följt hello rekommendation hello varningsvillkor lokala du bör ta bort hello avisering från hello enhet genom att välja hello avisering i hello **aviseringar** fliken och klicka på **Rensa**. tooclear flera aviseringar, väljer varje varning, klicka på alla kolumner utom hello **avisering** kolumnen och klicka sedan på **Rensa** när du har valt alla hello aviseringar toobe avmarkerad. Observera att vissa aviseringar tas bort automatiskt när hello problemet är löst eller hello systemet uppdaterar hello avisering med ny information.

När du klickar på **Rensa**, måste hello möjlighet tooprovide kommentarer om hello aviseringen och hello steg som du tog tooresolve hello problemet. Vissa händelser rensas hello systemet om en annan händelse utlöses med ny information. I så fall visas följande meddelande hello.

![Ta bort meddelande](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Sortera och granska aviseringar
Det kan vara effektivare toorun rapporter om aviseringar så att du kan granska och rensa dem i grupper. Dessutom hello **aviseringar** fliken Visa in too250 aviseringar. Om du har överskridit det antalet aviseringar visas inte alla aviseringar i hello standardvyn. Du kan kombinera hello följande fält toocustomize aviseringar visas:

* **Status för** – du kan visa antingen **Active** eller **avmarkerad** aviseringar. Aktiva aviseringar aktiveras fortfarande i systemet, medan avmarkerad aviseringar antingen har rensats manuellt av en administratör eller programmatiskt eftersom hello system uppdatera hello varningsvillkor med ny information.
* **Allvarlighetsgrad** – du kan visa aviseringar för alla allvarlighetsgrader (kritisk, varning, information) eller bara en viss allvarlighetsgrad, till exempel endast kritiska aviseringar.
* **Källan** – du kan visa aviseringar från alla källor eller begränsa hello aviseringar toothose som kommer från antingen hello service eller en eller alla hello-enheter.
* **Tidsintervallet** – genom att ange hello **från** och **till** datum och tidsstämplar du tittar på aviseringar under hello tidsperiod som du är intresserad av.

## <a name="alerts-quick-reference"></a>Snabbreferens för aviseringar
hello följande tabeller listar några av hello Microsoft Azure StorSimple aviseringar som kan uppstå, samt ytterligare information och rekommendationer i förekommande fall. StorSimple-enhet aviseringar indelas i följande kategorier hello:

* [Molnet anslutningsvarningar](#cloud-connectivity-alerts)
* [Klustret aviseringar](#cluster-alerts)
* [Disaster recovery aviseringar](#disaster-recovery-alerts)
* [Maskinvara aviseringar](#hardware-alerts)
* [Jobbet aviseringar](#job-failure-alerts)
* [Lokalt Fäst volym aviseringar](#locally-pinned-volume-alerts)
* [Aviseringar för nätverk](#networking-alerts)
* [Prestandavarningar](#performance-alerts)
* [Säkerhetsaviseringar](#security-alerts)
* [Stöd för paketet aviseringar](#support-package-alerts)

### <a name="cloud-connectivity-alerts"></a>Molnet anslutningsvarningar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Anslutning för <*autentiseringsuppgifter molnnamn*> kan inte upprättas. |Det går inte att ansluta toohello storage-konto. |Det verkar som om det kan finnas ett anslutningsproblem med din enhet. Kör hello `Test-HcsmConnection` cmdlet från hello gränssnittet i Windows PowerShell för StorSimple på din enhet tooidentify och åtgärda hello problem. Om hello-inställningarna är korrekta kanske hello problemet med hello autentiseringsuppgifterna för hello storage-konto som hello varningen skapades. I det här fallet använder hello `Test-HcsStorageAccountCredential` cmdlet toodetermine om det finns problem som du kan lösa.<ul><li>Kontrollera nätverksinställningarna.</li><li>Kontrollera autentiseringsuppgifterna för ditt lagringskonto.</li></ul> |
| Vi har inte tagit emot ett pulsslag från din enhet för hello senast <*nummer*> minuter. |Det går inte att ansluta toodevice. |Det verkar som om det finns ett anslutningsproblem med din enhet. Använd hello `Test-HcsmConnection` cmdlet från hello gränssnittet i Windows PowerShell för StorSimple på din enhet tooidentify och åtgärda problem hello eller kontakta nätverksadministratören. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>StorSimple-beteendet när molnet anslutningen misslyckas
Vad händer om molnet anslutningen misslyckas för enheten StorSimple körs i produktion?

Om molnet anslutningen misslyckas på din StorSimple-enhet för produktion, sedan inträffa beroende hello tillståndet för din enhet hello följande: 

* **För hello lokala data på enheten**: under en viss tid, finns inga avbrott och läser fortsätter toobe hanteras. Hello Antal utestående I/o, ökar och överskrider en gräns kunde hello läsningar starta toofail. 
  
    Beroende på hello mängden data på enheten fortsätter hello skrivningar också toooccur för hello första några timmar efter hello avbrott i hello molnet anslutningen. hello skrivningar kommer sedan långsammare och slutligen starta toofail om hello molnet anslutning avbryts under flera timmar. (Det finns tillfällig lagring på hello enhet för data som är toobe pushas toohello moln. Det här området rensas när hello data skickas. Om anslutningen misslyckas, data i den här lagringsutrymmet pushas inte toohello molnet och IO misslyckas.)   
* **För hello data i molnet hello**: för de flesta molnet anslutningsfel, returneras ett fel. När hello anslutningen har återställts återupptas hello IOs utan hello användare med toobring hello volym online. I sällsynta fall kanske åtgärder från användaren krävs toobring tillbaka hello volymen online från hello klassiska Azure-portalen. 
* **För molnögonblicksbilder pågår**: hello åtgärden försöks några gånger inom 4 – 5 timmar och om hello anslutningen inte återställs, hello molnögonblicksbilder misslyckas.

### <a name="cluster-alerts"></a>Klustret aviseringar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Enheten växlar över för <*enhetsnamn*>. |Enheten är i underhållsläge. |Enheten växlar över på grund av tooentering eller befintliga underhållsläge. Detta är normalt och ingen åtgärd krävs. När du har godkänt den här aviseringen, avmarkerar du den från hello aviseringar sida. |
| Enheten växlar över för <*enhetsnamn*>. |Enhetens inbyggda programvara eller programvara har precis uppdaterats. |Det uppstod ett kluster på grund av tooan uppdatering. Detta är normalt och ingen åtgärd krävs. När du har godkänt den här aviseringen, avmarkerar du den från hello aviseringar sida. |
| Enheten växlar över för <*enhetsnamn*>. |Domänkontrollanten stängdes av eller startas om. |Det gick inte över enheten eftersom hello aktiva styrenheten stängdes av eller startas om av en administratör. Ingen åtgärd krävs. När du har godkänt den här aviseringen, avmarkerar du den från hello aviseringar sida. |
| Enheten växlar över för <*enhetsnamn*>. |Planerad redundans. |Kontrollera att det var en planerad redundans. Rensa aviseringen från hello aviseringar sida när du har vidtagit lämpliga åtgärder. |
| Enheten växlar över för <*enhetsnamn*>. |Oplanerad redundans. |StorSimple bygger tooautomatically Återställ från oplanerad växling vid fel. Kontakta Microsoft Support om du ser ett stort antal aviseringarna. |
| Enheten växlar över för <*enhetsnamn*>. |Andra/okänd orsak. |Kontakta Microsoft Support om du ser ett stort antal aviseringarna. När hello problem är löst, rensa den här aviseringen från hello aviseringar sida. |
| En tjänst för kritiska enheter rapporterar status som misslyckad. |DataPath tjänstfel. |Kontakta Microsoft Support för hjälp. |
| Den virtuella IP-adressen för nätverksgränssnittet <*DATA #*> rapporterar status som misslyckad. |Andra/okänd orsak. |Ibland temporära tillstånd kan orsaka aviseringarna. Om så är fallet hello sedan rensas aviseringen automatiskt efter en stund. Om hello problemet kvarstår kontaktar du Microsoft Support. |
| Den virtuella IP-adressen för nätverksgränssnittet <*DATA #*> rapporterar status som misslyckad. |Gränssnittsnamn: <*DATA #*> IP-adress <IP address> kunde inte försättas online eftersom en dubbel IP-adress har identifierats på hello nätverk. |Att hello duplicerade IP-adressen tas bort från hello nätverk eller konfigurera om hello gränssnitt med en annan IP-adress. |

### <a name="disaster-recovery-alerts"></a>Disaster recovery aviseringar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Återställningsåtgärder kunde inte återställa alla hello inställningar för den här tjänsten. Konfigurationsdata för enheten är i ett inkonsekvent tillstånd för vissa enheter. |Datainkonsekvens upptäcktes när katastrofåterställning. |Krypterade data på hello-tjänsten är inte synkroniserad med på hello enhet. Auktorisera hello enhet <*enhetsnamn*> från StorSimple Manager toostart hello synkroniseringsprocessen. Använd hello gränssnittet i Windows PowerShell för StorSimple toorun hello `Restore-HcsmEncryptedServiceData` på enhet <*enhetsnamn*> cmdlet, att ange hello gamla lösenord som en inkommande toothis cmdlet toorestore hello säkerhetsprofil. Kör sedan hello `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet tooupdate hello krypteringsnyckel för tjänstdata. Rensa aviseringen från hello aviseringar sida när du har vidtagit lämpliga åtgärder. |

### <a name="hardware-alerts"></a>Maskinvara aviseringar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Maskinvarukomponent <*komponent-ID*> rapporterar status som <*status*>. | |Ibland temporära tillstånd kan orsaka aviseringarna. I så fall, kommer aviseringen rensas automatiskt efter en stund. Om hello problemet kvarstår kontaktar du Microsoft Support. |
| Passiva domänkontrollanten fungerar inte. |hello fungerar passiva (sekundär) inte. |Enheten är i drift, men en av dina domänkontrollanter är fel. Försök att starta om den styrenheten. Om hello problemet kvarstår kontaktar du Microsoft Support. |
| Fel med nära förestående enheten identifieras. | Fel med nära förestående enheten identifieras. |Vi har upptäckt ett nära förestående enhet fel för hello maskinvarukomponent ' enhet i uttag <*fack ID*>, höljet <*hölje ID*>'. Överväg att byta ut din enhet. <br> Innan du börjar diskbyte hello, granska hello följande information.<br><br>Om enheten har mer än en felande disken, inte bort mer än en SSD och HDD när som helst. Detta kan resultera i förlust av data.<br><br>Se till att du placerar en ersättning SSD i en plats som tidigare har innehållit en SSD. hello sak samma gäller för en Hårddisk.<br><br>Platser numreras från 0 too11. En disk som misslyckats i uttag 2 mappar tooa felaktiga disken i uttag 3 av hello enhet (från hello övre vänstra).<br><br>Mer information om diskbyte gå toohttps://go.microsoft.com/fwlink/?linkid=838653. Om problemet kvarstår kontaktar du Microsoft support via https://go.microsoft.com/fwlink/?linkid=838654. |

### <a name="job-failure-alerts"></a>Jobbet aviseringar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Säkerhetskopiering av <*grupp-ID för volymen*> misslyckades. |Säkerhetskopieringsjobbet misslyckades. |Problem med nätverksanslutningen kan förhindra hello säkerhetskopieringen från en lyckad. Om det inte finns några anslutningsproblem, har du nått hello maximalt antal säkerhetskopior. Ta bort alla säkerhetskopieringar som inte längre behövs och försök utföra hello igen. Rensa aviseringen från hello aviseringar sida när du har vidtagit lämpliga åtgärder. |
| Klona av <*datakällan säkerhetskopiering element-ID: N*> för <*mål volym serienummer*> misslyckades. |Det gick inte att klona. |Uppdatera hello säkerhetskopia av listan tooverify hello säkerhetskopiering är fortfarande giltig. Om hello säkerhetskopieringen är giltiga, är det möjligt att molnet anslutningsproblem hindrar hello kopieringen från en lyckad. Om det inte finns några anslutningsproblem, har du nått hello lagringsgräns. Ta bort alla säkerhetskopieringar som inte längre behövs och försök utföra hello igen. När du har vidtagit lämpliga åtgärder tooresolve hello problemet, avmarkerar du den här aviseringen från hello aviseringar sida. |
| Återställningen av <*datakällan säkerhetskopiering element-ID: N*> misslyckades. |Det gick inte att återställa. |Uppdatera hello säkerhetskopia av listan tooverify hello säkerhetskopiering är fortfarande giltig. Om hello säkerhetskopieringen är giltiga, är det möjligt att molnet anslutningsproblem hindrar hello återställningen från en lyckad. Om det inte finns några anslutningsproblem, har du nått hello lagringsgräns. Ta bort alla säkerhetskopieringar som inte längre behövs och försök utföra hello igen. När du har vidtagit lämpliga åtgärder tooresolve hello problemet, avmarkerar du den här aviseringen från hello aviseringar sida. |

### <a name="locally-pinned-volume-alerts"></a>Lokalt Fäst volym aviseringar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Skapa lokal volym <*volymnamn*> misslyckades. |Det gick inte att hello jobbet för att skapa volymen. <*Fel meddelande motsvarande toohello misslyckades felkod*>. |Problem med nätverksanslutningen kan förhindra hello utrymme åtgärden för att skapa från en lyckad. Lokalt fästa volymer etableras tjockt, vilket och hello skapa utrymme innebär läcka nivåindelade volymer toohello moln. Om det inte finns några anslutningsproblem, kan du ha uttömt hello lokalt utrymme på hello enhet. Avgör om utrymme finns på hello enheten innan du gör den här åtgärden. |
| Utökningen av lokal volym <*volymnamn*> misslyckades. |hello volymen ändras jobbet misslyckades på grund av för <*fel meddelande motsvarande toohello misslyckades felkod*>. |Problem med nätverksanslutningen kan förhindra hello volym expansion har att slutföra åtgärden. Lokalt fästa volymer etableras tjockt, vilket och hello utöka hello befintliga utrymme innebär läcka nivåindelade volymer toohello moln. Om det inte finns några anslutningsproblem, kan du ha uttömt hello lokalt utrymme på hello enhet. Avgör om utrymme finns på hello enheten innan du gör den här åtgärden. |
| Konvertering av volymen <*volymnamn*> misslyckades. |Det gick inte att hello volym konvertering tooconvert hello volym jobbtypen från lokalt Fäst tootiered. |Konvertering av hello volym från typen lokalt Fäst tootiered kunde inte slutföras. Se till att det inte finns några anslutningsproblem som hindrar hello åtgärden slutförs. Felsökning av anslutningsmöjligheter problem finns för[felsökning med hello Test-HcsmConnection cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>hello ursprungliga lokalt Fäst volym har nu markerats som en nivåindelad volym eftersom vissa hello data från hello lokalt Fäst volym har hamnat toohello molnet under hello konverteringen. hello gällande nivåindelad volym fortfarande använder lokalt utrymme på hello enhet som kan vara frigöras för framtida lokala volymer.<br>Lös eventuella problem med nätverksanslutningen, ta bort hello aviseringen och konvertera den här volymen tillbaka toohello ursprungliga lokalt Fäst volym typen tooensure alla hello data görs tillgänglig lokalt igen. |
| Konvertering av volymen <*volymnamn*> misslyckades. |Det gick inte att hello volym konvertering tooconvert hello volym jobbtypen från nivåindelade toolocally Fäst. |Konvertering av hello volym från typen nivåindelade toolocally Fäst kunde inte slutföras. Se till att det inte finns några anslutningsproblem som hindrar hello åtgärden slutförs. Felsökning av anslutningsmöjligheter problem finns för[felsökning med hello Test-HcsmConnection cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>hello ursprungliga nivåindelad volym nu markerats som en lokalt Fäst volym som en del av hello konverteringen fortsätter toohave data som finns i hello molnet, medan hello etableras tjockt, vilket utrymme på hello enheten för den här volymen kan inte längre frigöras för framtida lokal volymer.<br>Lös eventuella problem med nätverksanslutningen, rensa hello aviseringen och konvertera den här volymen tillbaka toohello ursprungliga nivåindelad volym typen tooensure lokalt utrymme etableras tjockt, vilket på hello enhet kan vara frigöras. |
| Snart lokalt utrymme förbrukningen för lokala ögonblicksbilder av <*volym gruppnamn*> |Lokala ögonblicksbilder för hello säkerhetskopieringsprincip kan snart slut på utrymme och vara inaktuella tooavoid skrivfel för värden. |Ofta lokala ögonblicksbilder tillsammans med en hög dataomsättningen i hello volymer som är associerade med den här gruppen säkerhetskopieringsprincip orsakar lokalt utrymme på hello enheten toobe förbrukas snabbt. Ta bort alla lokala ögonblicksbilder som inte längre behövs. Dessutom uppdatera scheman lokal ögonblicksbild för den här säkerhetskopieringsprincip tootake mindre ofta lokala ögonblicksbilder och kontrollera att molnögonblicksbilder tas regelbundet. Om dessa åtgärder vidtas inte, lokala utrymmet för dessa ögonblicksbilder kan snart slut och hello system kommer automatiskt bort tooensure som värden skrivning fortsätta toobe har bearbetats. |
| Lokala ögonblicksbilder för <*volym gruppnamn*> är ogiltiga. |Hej lokala ögonblicksbilder för <*volym gruppnamn*> ogiltig och sedan tas bort eftersom de mer än hello lokalt utrymme på hello enhet. |tooensure detta inte upprepas i framtida hello, granska hello lokal ögonblicksbild scheman för denna säkerhetskopieringsprincip och ta bort alla lokala ögonblicksbilder som inte längre behövs. Ofta lokala ögonblicksbilder tillsammans med en hög dataomsättningen i hello volymer som är associerade med den här gruppen säkerhetskopieringsprincip orsaka lokalt utrymme på hello enheten toobe förbrukas snabbt. |
| Återställningen av <*datakällan säkerhetskopiering element-ID: N*> misslyckades. |hello återställningen misslyckades. |Om du har fäst lokalt eller en blandning av lokalt Fäst och nivåindelade volymer i denna princip för säkerhetskopiering, uppdatera hello säkerhetskopia av listan tooverify hello säkerhetskopiering fortfarande är giltigt. Om hello säkerhetskopieringen är giltiga, är det möjligt att molnet anslutningsproblem hindrar hello återställningen från en lyckad. hello lokalt fästa volymer som återställdes som en del av den här gruppen ögonblicksbild inte är alla sina data hämtade toohello enheter, och om du har en blandning av nivåindelade och lokalt fästa volymer i den här ögonblicksbilden-gruppen, kan de inte synkroniserade med varandra. toosuccessfully slutföra hello återställningen, tar hello volymer i den här gruppen offline på hello värden och försök hello återställningen. Observera att alla ändringar toohello volymdata som utfördes under hello återställningen kommer att förloras. |

### <a name="networking-alerts"></a>Aviseringar för nätverk
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Det gick inte att starta StorSimple-tjänst(er). |DataPath fel |Om hello problemet kvarstår kontaktar du Microsoft Support. |
| IP-adressdubblett upptäcktes för 'Data0'. | |hello systemet har upptäckt en konflikt för IP-adress ”10.0.0.1”. Hej nätverksresurs 'Data0' på hello enhet  *<device1>*  är offline. Se till att IP-adressen inte används av andra entiteter i nätverket. tootroubleshoot nätverksproblem gå för[felsökning med hello Get-NetAdapter cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Kontakta nätverksadministratören för hjälp med att lösa problemet. Om hello problemet kvarstår kontaktar du Microsoft Support. |
| IPv4 (eller IPv6) adress 'Data0' är offline. | |hello nätverksresurs 'Data0' med IP-adressen 10.0.0.1. och prefixlängd '22' på hello enhet  *<device1>*  är offline. Säkerställa att hello växeln portar toowhich som det här gränssnittet är anslutet fungerar. tootroubleshoot nätverksproblem gå för[felsökning med hello Get-NetAdapter cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |

### <a name="performance-alerts"></a>Prestandavarningar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| hello enheten belastningen har överskridit <*tröskelvärdet*>. |Långsammare än förväntat svarstider. |Enheten rapporterar användning under en hög i/o-belastning. Det kan leda till enheten toonot arbetet samt den borde. Granska hello arbetsbelastning som du har kopplat toohello enhet och avgöra om det finns några kunde flyttas tooanother enhet eller som inte längre behövs.<br>toounderstand Hej aktuell status, gå för[Använd hello StorSimple Manager service toomonitor enheten](storsimple-monitor-device.md) |

### <a name="security-alerts"></a>Säkerhetsaviseringar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Microsoft Support-sessionen har startat. |Tredjeparts-åt stöd session. |Bekräfta att du har denna behörighet. Rensa aviseringen från hello aviseringar sida när du har vidtagit lämpliga åtgärder. |
| Lösenordet för <*elementet*> upphör att gälla om <*tid*>. |Lösenordets giltighetstid närmar sig. |Ändra lösenordet innan det upphör att gälla. |
| Information om säkerhetskonfigurationen saknas för <*element-ID*>. | |Hej volymer som är associerade med volymbehållaren går inte att använda tooreplicate din StorSimple-konfiguration. tooensure att dina data lagras på ett säkert sätt, rekommenderar vi att du tar bort hello volymbehållare och alla volymer som är associerade med hello volymbehållare. Rensa aviseringen från hello aviseringar sida när du har vidtagit lämpliga åtgärder. |
| <*antal*> inloggningsförsök misslyckades för <*element-ID*>. |Flera inloggningsförsök misslyckade. |Enheten kan vara angripet eller en behörig användare försöker tooconnect med ett felaktigt lösenord.<ul><li>Kontakta din behöriga användare och kontrollera att dessa försök var från en säker källa. Överväg att inaktivera fjärrhantering och kontakta nätverksadministratören om du fortsätter toosee stort antal misslyckade inloggningsförsök. Rensa aviseringen från hello aviseringar sida när du har vidtagit lämpliga åtgärder.</li><li>Kontrollera att din Snapshot Manager-instanserna är konfigurerade med hello rätt lösenord. Rensa aviseringen från hello aviseringar sida när du har vidtagit lämpliga åtgärder.</li></ul>Mer information finns för[ändrar du ett enhetslösenord har upphört att gälla](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password). |
| En eller flera fel uppstod när ändra hello krypteringsnyckel för tjänstdata. | |Det uppstod fel påträffades när du ändrar hello krypteringsnyckel för tjänstdata. När du har åtgärdat hello felvillkor kör hello `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet från hello Windows PowerShell-gränssnitt för StorSimple på din enhet tooupdate hello-tjänst. Kontakta Microsoft-supporten om problemet kvarstår. När du har löst problemet hello avmarkerar du den här aviseringen från hello aviseringar sida. |

### <a name="support-package-alerts"></a>Stöd för paketet aviseringar
| Varningstexten | Händelse | Mer information / rekommenderade åtgärder |
|:--- |:--- |:--- |
| Det gick inte att skapa för stödpaketet. |StorSimple gick inte att generera hello-paketet. |Försök igen. Om hello problemet kvarstår kontaktar du Microsoft Support. När du har löst problemet hello, avmarkera den här aviseringen från hello aviseringar sida. |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [StorSimple fel och felsöka en operativa enhet](storsimple-troubleshoot-operational-device.md).

