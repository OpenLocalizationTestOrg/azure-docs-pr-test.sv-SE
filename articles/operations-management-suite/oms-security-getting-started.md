---
title: "aaaGetting igång med Operations Management Suite säkerhet och granska lösningen | Microsoft Docs"
description: "Det här dokumentet hjälper du tooget igång med Operations Management Suite säkerhet och granska lösningen funktioner toomonitor din hybridmoln."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 754796ef-a43e-468a-86c9-04a2eda55b5b
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: get-started-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 5cb3e5dbb3e60f9702a34c9413ddc1bf2b14b411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Komma igång med säkerhets- och granskningslösningen i Operations Management Suite
Det här dokumentet hjälper dig att snabbt komma igång med funktionerna i säkerhets- och granskningslösningen i Operations Management Suite (OMS) genom att vägleda dig igenom de olika alternativen.

## <a name="what-is-oms"></a>Vad är OMS?
Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur. Mer information om OMS artikeln hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Instrumentpanelen för Säkerhet och granskning i OMS
hello OMS säkerhet och granska lösningen innehåller en heltäckande översikt över din organisations IT säkerhetstillståndet med inbyggda sökfrågor för anmärkningsvärda problem som kräver din uppmärksamhet. Hej **säkerhet och granska** instrumentpanelen är hello startsidan för allt relaterade toosecurity i OMS. Det ger en övergripande inblick i hello säkerhetsstatus för dina datorer. Den omfattar också hello möjlighet tooview alla händelser från hello senaste 24 timmarna, 7 dagar eller andra anpassade tidsintervall. tooaccess hello **säkerhet och granska** instrumentpanel, gör du följande:

1. I hello **Microsoft Operations Management Suite** huvudsakliga instrumentpanelen klickar du på **inställningar** panelen hello till vänster.
2. I hello **inställningar** bladet under **lösningar** klickar du på **säkerhet och granska** alternativet.
3. Hej **säkerhet och granska** instrumentpanelen visas:
   
    ![Instrumentpanelen för Säkerhet och granskning i OMS](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Om du ansluter till den här instrumentpanelen för hello första gången och du inte har enheter som övervakas av OMS, fylls inte hello paneler med data som hämtats från hello agent. När du har installerat agenten hello det kan ta viss tid toopopulate, därför ser du ursprungligen kan sakna vissa data som de överförs fortfarande toohello moln.  I det här fallet är det normala toosee vissa rutor utan konkreta information. Läs [ansluta Windows-datorer direkt tooOMS](https://technet.microsoft.com/library/mt484108.aspx) för mer information om hur tooinstall OMS-agent i ett Windows-system och [ansluta Linux-datorer tooOMS](https://technet.microsoft.com/library/mt622052.aspx) för mer information om hur tooperform den här aktiviteten i ett Linux-system.

> [!NOTE]
> hello agenten samlar in hello information baserat på hello aktuella händelser som är aktiverade, till exempel datornamn, IP-adress och användarens namn. Inga dokument/filer, databasnamn eller privata data samlas dock in.   
> 
> 

Lösningar är en samling logik, visualisering och datahämtningsregler som hjälper till att hantera viktiga kundutmaningar. Säkerhet och granskning är en lösning, andra kan läggas till separat. Läs hello artikel [lägga till lösningar](https://technet.microsoft.com/library/mt674635.aspx) för mer information om hur tooadd en ny lösning.

hello OMS säkerhet och granska instrumentpanelen är organiserat i fyra huvudsakliga kategorier:

* **Säkerhetsdomäner**: i det här området kommer du att kunna toofurther utforska säkerhetsposter över tid, gå till kontroll av skadlig kod, uppdatera assessment, nätverkssäkerhet, identitet och åtkomst, datorer med säkerhetshändelser och snabbt har instrumentpanelen för tooAzure Security Center.
* **Anmärkningsvärda problem**: det här alternativet tillåter tooquickly identifiera hello antal aktiva problem och hello allvarlighetsgrad för de här problemen.
* **Identifieringar (förhandsgranskning)**: du kan använda tooidentify angreppsmönster genom visualisera säkerhetsaviseringar som de ske mot dina resurser.
* **Hot Intelligence**: du kan använda tooidentify angreppsmönster genom visualisera hello Totalt antal servrar med utgående skadlig IP-trafik, hello skadliga hot typ och en karta som visar var dessa IP-adresser kommer från. 
* **Vanliga säkerhetsfrågor**: det här alternativet får du en lista över de vanligaste hello-säkerhet frågor som du kan använda toomonitor din miljö. När du klickar på någon av dessa frågor öppnas hello **Sök** bladet med hello resultat för frågan.

> [!NOTE]
> Mer information om hur OMS skyddar dina data finns i Så här skyddas dina data med OMS.
> 
> 

## <a name="security-domains"></a>Säkerhetsdomäner
När övervakning resurser, är det viktigt toobe kan tooquickly åtkomst hello aktuell status för din miljö. Dock är det också viktigt toobe kan tootrack tillbaka händelser som inträffade i hello senaste som kan leda till tooa bättre förståelse för vad som händer i din miljö på en viss punkt i tiden. 

> [!NOTE]
> datalagring sker enligt toohello OMS prisavtal. Mer information finns i hello [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) sida med priser.
> 
> 

Incident svar och dataforensik undersökning scenarier direkt drar nytta av hello resultat är tillgängliga i hello **säkerhetsposter över tid** panelen.

![Säkerhetsposter över tid](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

När du klickar på den här panelen hello **Sök** öppnas bladet, visar ett frågeresultat för **säkerhetshändelser** (typ = SecurityEvents) med data baserat på hello senaste sju dagarna, enligt nedan:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Säkerhetsposter över tid](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

hello sökresultatet är uppdelad i två rutor: hello vänster ger en detaljerad analys av hello antalet säkerhetshändelser som har hittats, hello datorer där händelserna hittades, hello antalet konton som har identifierats i dessa datorer och hello typer av aktiviteter. hello till höger innehåller hello totalt resultat och en kronologisk vy av hello säkerhetshändelser med hello datorns namn och en händelse aktivitet. Du kan också klicka på **visa fler** tooview mer information om den här händelsen, till exempel hello händelsedata, hello händelse-ID och hello händelsekälla.

> [!NOTE]
> Mer information om OMS-sökfrågor finns i [sökreferensen för OMS](https://technet.microsoft.com/library/mt450427.aspx).
> 
> 

### <a name="antimalware-assessment"></a>Utvärdering av program mot skadlig kod
Det här alternativet kan du tooquickly identifiera datorer med otillräckligt skydd och datorer som har angripits av en typ av skadlig kod. Kontroll av skadlig kod status hot har identifierats på hello övervakade servrar är skrivskyddade och hello data skickas toohello OMS-tjänsten i hello molnet för bearbetning. Servrar med identifierade hot och servrar utan tillräckligt skydd visas i hello skadlig kod assessment instrumentpanelen, som är tillgänglig när du klickar på hello **program mot skadlig kod Assessment** panelen. 

![utvärdering av skadlig kod](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Precis som alla andra levande sida vid sida finns i OMS-instrumentpanelen, när du klickar på den hello **Sök** öppnas bladet hello frågeresultatet. För det här alternativet om du klickar i hello **inte rapporterar** alternativ **skyddsstatus**, har du hello frågeresultat som visar den här enda post som innehåller hello datornamnet och rangen som visas nedan:

![sökresultat](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [!NOTE]
> *rangordning* är en klass som ger tooreflect hello status för hello skydd (på, avstängd, uppdatera, osv) och hot som hittas. Har som som en nummer hjälper toomake aggregeringar.
> 
> 

Om du klickar på hello datornamn, har du hello kronologisk vy över hello skyddsstatus för den här datorn. Detta är användbart för scenarier där du behöver toounderstand om hello program mot skadlig kod har varit installerat och det togs bort på någon punkt.   

### <a name="update-assessment"></a>Kontroll av uppdateringar
Det här alternativet kan du tooquickly fastställa hello exponeringen toopotential säkerhetsproblem, och huruvida eller hur kritiska de här uppdateringarna är för din miljö. OMS säkerhets- och granska lösningen och ge endast hello visualisering av dessa uppdateringar, hello verkliga data som kommer från [uppdatering hanteringslösningar](oms-solution-update-management.md), vilket är en annan modul i OMS. Ett exempel på hello uppdateringar här:

![systemuppdateringar](./media/oms-security-getting-started/oms-getting-started-fig6-new.png)

> [!NOTE]
> Mer information om lösningar för uppdateringshantering finns i [Uppdateringshanteringslösning i OMS](oms-solution-update-management.md).
> 
> 

### <a name="identity-and-access"></a>Identitet och åtkomst
Identiteten ska vara hello kontrollen plan för ditt företag, skydda din identitet ska vara din högsta prioritet. Även i hello som tidigare fanns ytgränser runt organisationer och dessa ytgränser har en hello primära defensiva gränser, blir Nuförtiden med mer data och fler appar flytta toohello moln hello identitet hello nya perimeternätverk. 

> [!NOTE]
> för närvarande hello baseras enbart på säkerhetshändelser inloggningsdata (händelse-ID 4624) i hello framtida Office365 inloggningar och Azure AD-data kommer att visas.
> 
> 

Du kommer att kunna tootake förebyggande åtgärder innan en incident äger rum eller reaktiv åtgärder toostop ett angrepp försök genom att övervaka dina identity-aktiviteter. Hej **identitets- och** instrumentpanelen ger en översikt över din identitet tillstånd, inklusive hello antalet misslyckade försök toolog på hello-konto som användes vid sådana försök konton som har låst konton med ändrat eller återställt lösenord och antalet konton som är inloggad för närvarande. 

Om du klickar på hello **identitets- och** panelen visas hello följande instrumentpanelen:

![identitet och åtkomst](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

hello informationen i den här instrumentpanelen hjälper omedelbart tooidentify potentiella misstänkt aktivitet. Det är exempelvis 338 försök toolog på som **administratör** och 100% av dessa försök misslyckades. Detta kan bero på en råstyrkeattack mot kontot. Om du klickar på det här kontot kommer du få mer information som kan hjälpa dig toodetermine hello målresursen för potentiella angrepp:

![sökresultat](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

hello detaljerad rapport innehåller viktig information om den här händelsen, inklusive: hello måldatorn, hello typ av inloggning (i det här fallet nätverksinloggning), hello aktivitet (i det här fallet händelse 4625) och en omfattande tidslinjen för varje försök. 

### <a name="computers"></a>Datorer
Den här panelen kan vara används tooaccess alla datorer som aktivt har säkerhetshändelser. Om du klickar på den här panelen visas hello lista över datorer med säkerhetshändelser och hello antalet händelser på varje dator:

![Datorer](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Du kan fortsätta undersökningen genom att klicka på varje dator och granska hello säkerhetshändelser som har flaggats.

### <a name="threat-intelligence"></a>Hotinformation

Med hjälp av hello Hotinformation alternativ som finns i OMS-säkerhet och granska IT-administratörer identifierar säkerhetshot mot hello miljö, till exempel, identifiera om en viss dator är en del av ett botnät. Datorer kan bli noder i ett botnät när angripare otillåtet sätt installera skadlig kod som någon gång ansluter den här datorn toohello kommando och kontroll. Hotinformation kan också identifiera potentiella hot från kommunikationskanaler under marken, som darknet. Mer information om Hotinformation genom att läsa [övervaknings- och svarar toosecurity aviseringar i Operations Management Suite säkerhet och granska lösningen](oms-security-responding-alerts.md) artikel.

I vissa situationer kan finnas det en potentiellt skadlig IP-adress som öppnas från en övervakad dator:

![hotinformationkarta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Den här aviseringen och andra inom Hej samma kategori, genereras via OMS säkerhet genom att utnyttja [Microsoft Hotinformation](https://youtu.be/O4WtxgUrDc8). Hej Hotinformation data är samlas in av Microsoft som köpts från inledande hot intelligence providers. Dessa data uppdateras ofta och anpassade toofast flytta hot. På grund av tooits karaktär, bör det kombineras med andra informationskällor säkerhet vid [undersöker](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) en säkerhetsavisering. 

### <a name="baseline-assessment"></a>Utvärdering av baslinje

Microsoft definierar, i samarbete med branschspecifika och offentliga organisationer i hela världen, en Windows-konfiguration för mycket säkra serverinstallationer. Den här konfigurationen består av en uppsättning registernycklar, granskningsprincipinställningar och säkerhetsprincipinställningar tillsammans med Microsofts rekommenderade värden för dessa inställningar. Den här uppsättningen regler kallas för en säkerhetsbaslinje. Läs [bedömning av baslinjen i Operations Management Suite säkerhet och granska lösningen](oms-security-baseline.md) för mer information om det här alternativet.

### <a name="azure-security-center"></a>Azure Security Center
Den här panelen är i princip en genväg tooaccess Azure Security Center instrumentpanel. Mer information om den här lösningen finns i [Komma igång med Azure Security Center](../security-center/security-center-get-started.md).

## <a name="notable-issues"></a>Anmärkningsvärda problem
hello huvudsakliga syftet med den här gruppen av alternativen är tooprovide en snabb överblick över hello problem som du har i din miljö genom att kategorisera dem i kritisk, varning och information. Hej aktivt ärende typen sida vid sida är det en visualisering av dessa fel, men den inte kan du tooexplore mer information om dem, som du behöver toouse hello nedre delen av den här panelen hello namnet hello-problemet (namn), hur många objekten hade detta (antal) och hur viktigt det är (ALLVARLIGHETSGRAD).

![Anmärkningsvärda problem](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Du kan se att de här problemen redan omfattas i olika områden av hello **säkerhetsdomäner** grupp som förstärker hello syftet med den här vyn: visualisera hello mest viktiga problem i miljön från en enda plats.

## <a name="detections-preview"></a>Identifieringar (förhandsversion)
hello huvudsakliga syftet med det här alternativet är tooallow IT tooquickly identifiera potentiella hot tootheir miljön via och hello allvarlighetsgrad för det här hotet.

![Hotinformation](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Det här alternativet kan också användas under en [incidenter undersökningen](https://blogs.msdn.microsoft.com/azuresecurity/2016/11/30/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) tooperform hello bedömning och få mer information om hello-attack.

> [!NOTE]
> Mer information om hur toouse OMS för Incident svar, se den här videon: [hur tooLeverage hello Azure Security Center & Microsoft Operations Management Suite för en Incident svar](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).
> 
> 

## <a name="threat-intelligence"></a>Hotinformation
Hej nya hot intelligence avsnitt av hello säkerhet och granska lösningen visualizes hello möjliga angreppsmönster på flera sätt: Hej Totalt antal servrar med utgående skadlig IP-trafik, hello skadliga hot typ och en karta som visar var du dessa IP-adresser kommer från. Du kan interagera med hello kartan och klicka på hello IP-adresser för mer information.

Gult kartnålar på hello karta visar inkommande trafik från skadliga IP-adresser. Det är inte ovanligt för servrar som är exponerade toohello internet toosee inkommande skadlig trafik, men vi rekommenderar att du granskar dessa försök toomake att ingen av dem har slutförts. Dessa indikatorer baseras på IIS-loggar, WireData och loggar för Windows-brandväggen.  

![Hotinformation](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Vanliga säkerhetsfrågor
hello lista över vanliga säkerhetsfrågor kan vara användbart för du toorapidly åtkomst resursens information och anpassa den utifrån behoven i din miljö. Dessa vanliga frågor är:

* Alla säkerhetsaktiviteter
* Säkerhetsaktiviteter på hello datorn \"computer01.contoso.com\” (Ersätt med namn på din dator)
* Säkerhetsaktiviteter på hello dator ”computer01.contoso.com” för kontot ”Administratör” (Ersätt med namn på din dator och kontonamn)
* Inloggningsaktivitet per dator
* Konton som avslutat Microsoft-program mot skadlig kod på en dator
* Datorer där hello process för Microsoft mot skadlig kod avslutades
* Datorer där ”hash.exe” har körts (ersätt namnet med processnamnet)
* Alla processnamn som körts
* Inloggningsaktivitet per konto
* Konton som har loggat in via fjärranslutning på hello datorn \"computer01.contoso.com\” (Ersätt med namn på din dator)

## <a name="see-also"></a>Se även
I det här dokumentet har introducerades tooOMS säkerhet och granska lösningen. toolearn mer om OMS-säkerhet finns hello följande artiklar:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning](oms-security-responding-alerts.md)
* [Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite](oms-security-monitoring-resources.md)

