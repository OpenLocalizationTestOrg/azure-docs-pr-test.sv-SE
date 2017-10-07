---
title: "aaaSecurity övervakning i Azure Security Center | Microsoft Docs"
description: "Den här artikeln hjälper tooget igång med övervakningsfunktionerna i Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: yurid
ms.openlocfilehash: 43c2a8864d5fe27ba44b0d7bc979db970305ec17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Övervakning av säkerhetshälsa i Azure Security Center
Den här artikeln kan du använda hello övervakningsfunktionerna i Azure Security Center toomonitor följer principerna.

## <a name="what-is-security-health-monitoring"></a>Vad är övervakning av säkerhetshälsa?
Ofta tänker vi övervakning och titta och vänta en händelse toooccur så att vi kan reagera toohello situation. Säkerhetsövervakning handlar toohaving en proaktiv strategi där granskningar dina resurser tooidentify system som inte uppfyller organisationens normer och bästa praxis.

## <a name="monitoring-security-health"></a>Övervakning av säkerhetshälsa
När du har aktiverat [säkerhetsprinciper](security-center-policies.md) för en prenumeration resurser, Security Center analyserar hello säkerheten för dina resurser tooidentify potentiella säkerhetsproblem. Information om nätverkskonfigurationen är tillgänglig direkt. Det kan ta en timme eller mer information om konfiguration av virtuell dator, till exempel säkerhet uppdatera status och konfiguration av operativsystem, toobecome som är tillgängliga. Du kan visa hello säkerhetsstatus för dina resurser och eventuella problem i hello **förebyggande** avsnitt. Du kan också visa en lista över dessa problem på hello **rekommendationer** panelen.

Mer information om hur tooapply rekommendationer läsa [utföra säkerhetsrekommendationerna i Azure Security Center](security-center-recommendations.md).

Under hello **förebyggande** avsnitt, kan du övervaka hello säkerhetsstatus för dina resurser. I följande exempel hello, ser du att varje resurs panelen (beräkning, nätverk, lagring och data och program) har hello Totalt antal problem som har identifierats.

![Panelen resurssäkerhetshälsa](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute"></a>Övervaka beräkning
När du klickar på **Compute** panelen, hello **Compute** bladet som öppnas visar tre flikar:

- **Översikt**: Övervakning och rekommendationer för virtuella datorer.
- **Virtuella datorer**: Lista över alla virtuella datorer och det aktuella säkerhetstillståndet.
- **Cloud Services**: Lista över alla webb- och arbetsroller som övervakas av Security Center.

![Systemuppdatering av den virtuella datorn saknas](./media/security-center-monitoring/security-center-monitoring-fig1-new002-2017.png)

Du kan ha flera avsnitt på varje flik och i varje avsnitt kan du välja ett enskilt alternativ toosee mer information om hello rekommenderade steg tooaddress viss problemet. 

#### <a name="monitoring-recommendations"></a>Rekommendationer för övervakning
Detta avsnitt visar hello Totalt antal virtuella datorer som initierats för datainsamling och deras aktuella status. När alla virtuella datorer datainsamling har initierats, kommer de att redo tooreceive säkerhetsprinciper från security Center. När du klickar på den här posten hello **VM-agenten saknas eller inte svarar** blad öppnas. 

![Systemuppdatering av den virtuella datorn saknas](./media/security-center-monitoring/security-center-monitoring-fig1-new003-2017.png)


#### <a name="virtual-machine-recommendations"></a>Rekommendationer för virtuella datorer
I den här delen finns ett antal [rekommendationer för de virtuella datorer](security-center-virtual-machine-recommendations.md) som övervakas via Azure Security Center. hello första kolumnen visar hello rekommendation. hello andra kolumnen visar hello Totalt antal virtuella datorer som påverkas av aktuell rekommendation. hello tredje kolumnen visar hello allvarlighetsgraden hello problemet enligt beskrivningen i följande skärmbild hello.

![Rekommendationer för virtuella datorer](./media/security-center-monitoring/security-center-monitoring-fig1-new004-2017.png)

> [!NOTE]
> Endast virtuella datorer som har minst en offentlig slutpunkt som visas i hello **nätverk hälsa** bladet i hello **nätverkstopologi** lista.
>
>

Varje rekommendation har en uppsättning åtgärder som du kan utföra när du klickar på den. Till exempel om du klickar på **saknas systemuppdateringar**, hello **saknas systemuppdateringar** blad öppnas. Visar hello virtuella datorer som saknar korrigeringsfiler och hello allvarlighetsgraden hello saknad uppdatering som visas i hello följande skärmbild.

![Saknade systemuppdateringar för virtuella datorer](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

Hej **saknas systemuppdateringar** bladet visas en tabell med hello följande information:

* **VIRTUELLA**: hello namnet på hello virtuell dator som saknar uppdateringar.
* **SYSTEMUPPDATERINGAR**: hello antalet systemuppdateringar som saknas.
* **SENASTE GENOMSÖKNINGSTID**: hello tid att senast genomsöktes hello virtuella datorn efter uppdateringar.
* **TILLSTÅND**: hello aktuell status för hello rekommendation:
  * **Öppna**: hello rekommendation har ännu inte utförts.
  * **Pågående**: hello rekommendationen håller tillämpade toothose resurser och ingen åtgärd krävs av dig.
  * **Matcha**: hello rekommendationen har redan slutförts. (När hello problemet har lösts hello-posten är nedtonad).
* **ALLVARLIGHETSGRAD**: Beskriver hello allvarlighetsgraden viktig rekommendationen:
  * **Hög**: Det finns en säkerhetsrisk i en viktig resurs (program, virtuell dator eller nätverkssäkerhetsgrupp) som måste åtgärdas.
  * **Medel**: icke-kritiska eller ytterligare steg är nödvändiga toocomplete en process eller åtgärda en säkerhetsrisk.
  * **Low (Låg)**: Det finns en säkerhetsrisk som bör åtgärdas, men det måste inte göras omedelbart. (Som standard låg rekommendationer inte visas, men du kan filtrera på låg rekommendationer om du vill tooview dem.)

tooview hello rekommendation information Klicka hello namnet på hello virtuell dator. Ett nytt blad för den virtuella datorn öppnar hello lista med uppdateringar som visas i följande skärmbild hello.

![Uppdateringar för en specifik virtuell dator saknas](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> hello säkerhetsrekommendationerna här är hello samma som i hello **rekommendationer** bladet. Se hello [utföra säkerhetsrekommendationerna i Azure Security Center](security-center-recommendations.md) artikel för mer information om hur tooresolve rekommendationer. Detta gäller inte bara för virtuella datorer utan även för alla resurser som är tillgängliga i hello **Resource Health** panelen.
>
>

#### <a name="virtual-machines-section"></a>Delen Virtuella datorer
hello virtuella datorer avsnittet ger en översikt över alla virtuella datorer och rekommendationer. Varje kolumn representerar en uppsättning rekommendationer som visas i följande skärmbild hello:

![Översikt över alla virtuella datorer och rekommendationer](./media/security-center-monitoring/security-center-monitoring-fig1-new005-2017.png)

hello-ikon som visas under varje rekommendation hjälper du tooquickly identifiera hello virtuella datorer som behöver åtgärdas och hello typ av rekommendation.

I föregående exempel hello har en virtuell dator en viktig rekommendation angående endpoint protection. tooget mer information om hello virtuell dator, klicka på den. Ett nytt blad öppnas representerar den här virtuella datorn som visas i följande skärmbild hello.

![Säkerhetsdetaljer för virtuella datorer](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Det här bladet har hello säkerhetsinformation för hello virtuella datorn. Du kan se hello rekommenderade åtgärder och hello allvarlighetsgraden varje problemet hello längst ned på bladet i.

#### <a name="cloud-services-section"></a>Avsnittet Molntjänster
För molntjänster skapas en rekommendation när hello operativsystemets version är för gammal som visas i följande skärmbild hello:

![Hälsostatus för molntjänster](./media/security-center-monitoring/security-center-monitoring-fig1-new006-2017.png)

I ett scenario där du har rekommendation (som inte är hello fallet för hello föregående exempel), måste toofollow hello stegen i hello rekommendation tooupdate hello operativsystemets version. När en uppdatering är tillgänglig, har du en avisering (röd eller orange - beror på hello allvarlighetsgraden hello problemet). När du klickar på den här aviseringen i hello WebRole1 (kör Windows Server med din web app distribueras automatiskt tooIIS) eller WorkerRole1 (kör Windows Server med din web app distribueras automatiskt tooIIS) rader öppnas ett nytt blad med mer information om detta rekommendation som visas i följande skärmbild hello:

![Molntjänstinformation](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

toosee en mer ingående förklaring av denna rekommendation klickar du på **uppdatering OS-version** under hello **beskrivning** kolumn. Hej **uppdatering OS-version (förhandsgranskning)** öppnas i blad med mer information.

![Molntjänstrekommendationer](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Övervakning av virtuella nätverk
När du klickar på **nätverk** panelen, hello **nätverk** öppnas i blad med mer information som visas i följande skärmbild hello:

![Bladet Nätverk](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Nätverksrekommendationer
Som hello hälsoinformation för virtuell dator resursen, innehåller det här bladet en sammanfattande lista över problem på hello överkant hello bladet och en lista med övervakade nätverk om hello längst ned.

hello nätverk uppdelning i avsnittet status visas potentiella säkerhetsproblem och erbjuder [rekommendationer](security-center-network-recommendations.md). Följande säkerhetsproblem kan visas:

* Nästa generations brandvägg inte installerad
* Nätverkssäkerhetsgrupper i undernät inte aktiverade
* Nätverkssäkerhetsgrupper på virtuella datorer är inte aktiverade
* Begränsa extern åtkomst genom offentlig extern slutpunkt
* Felfria internetuppkopplade slutpunkter

När du klickar på en rekommendation öppnas ett nytt blad med mer information om hello rekommendation som visas i följande exempel hello.

![Information om en rekommendation hello nätverk-bladet](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

I det här exemplet hello **konfigurera saknade Nätverkssäkerhetsgrupper för undernät** bladet har en lista över undernät och virtuella datorer som saknas network security group skydd. Om du klickar på hello undernät toowhich som du vill tooapply hello nätverkssäkerhetsgruppen öppnas ett nytt blad.

I hello **Välj nätverkssäkerhetsgrupp** bladet kan du välja hello mest lämpade nätverkssäkerhetsgruppen för undernätet hello eller du kan skapa en ny nätverkssäkerhetsgrupp.

#### <a name="internet-facing-endpoints-section"></a>Delen med internetuppkopplade slutpunkter
I hello **Internet facing endpoints** avsnittet kan du se hello virtuella datorer som är konfigurerade med en Internetuppkopplad slutpunkt och aktuell status.

![Virtuella datorer som konfigurerats med internetuppkopplad slutpunkt och status](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Den här tabellen har namnet på slutpunkten hello som representerar hello virtuella hello IP-adress mot Internet och hello aktuell allvarlighetsgrad för nätverkssäkerhetsgruppen hello och hello nästa generations Brandvägg. hello tabellen är sorterad efter allvarlighetsgrad:

* Röd (högst upp): hög prioritet och bör åtgärdas omedelbart
* Orange: medelhög prioritet och bör åtgärdas så snart som möjligt
* Grön (längst ned): god status

#### <a name="networking-topology-section"></a>Delen med nätverkstopologi
Hej **nätverkstopologi** avsnitt innehåller en hierarkisk vy över hello resurser som visas i följande skärmbild hello:

![Hierarkisk visning över resurser i avsnittet om nätverkstopologi](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Den här tabellen är sorterad (virtuella datorer och undernät) efter allvarlighetsgrad:

* Röd (högst upp): hög prioritet och bör åtgärdas omedelbart
* Orange: medelhög prioritet och bör åtgärdas så snart som möjligt
* Grön (längst ned): god status

I den här topologiska vyn hello första nivån har [virtuella nätverk](../virtual-network/virtual-networks-overview.md), [virtuella nätverksgatewayerna](/vpn-gateway/vpn-gateway-site-to-site-create.md), och [virtuella nätverk (klassiskt)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md). hello andra nivån har undernät och hello tredje nivån har hello virtuella datorer som tillhör toothose undernät. hello högra kolumnen innehåller hello aktuell status för hello nätverkssäkerhetsgruppen för de resurserna som visas i följande exempel hello:

![Status för hello nätverkssäkerhetsgruppen i delen med nätverkstopologi](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

hello längst ned i det här bladet har hello rekommendationer för den virtuella datorn som är liknande toowhat beskrivs ovan. Du kan klicka på en rekommendation toolearn mer eller använda hello behövs säkerhetskontroll eller konfiguration.

### <a name="monitor-storage--data"></a>Övervaka lagring och data

När du klickar på **lagring & data** i hello **förebyggande** avsnittet hello **dataresurser** blad öppnas med rekommendationer för SQL och lagring. Det har även [rekommendationer](security-center-sql-service-recommendations.md) för hello allmänna hälsotillstånd hello-databasen. Mer information om lagringskryptering finns i [Aktivera kryptering för Azure-lagringskontot i Azure Security Center](security-center-enable-encryption-for-storage-account.md).

![Dataresurser](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

Under **SQL rekommendationer**, kan du klicka på en rekommendation och få mer information om ytterligare åtgärd tooresolve ett problem. hello följande exempel visar hello expandering av hello **Database Auditing & Threat detection på SQL-databaser** rekommendation.

![Information om en SQL-rekommendation](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Hej **aktivera Auditing & Threat detection på SQL-databaser** bladet har hello följande information:

* en lista med SQL-databaser
* hello-server där de är placerade
* Information om huruvida inställningen har ärvts från hello server eller om det är unikt i den här databasen
* hello aktuella tillstånd
* hello allvarlighetsgraden hello problemet

När du klickar på hello databasen tooaddress rekommendationen hello **Auditing & Threat detection** blad öppnas enligt hello följande skärm.

![Bladet Granskning och hotidentifiering](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

tooenable granskning, Välj **ON** under hello **granskning** alternativet.

### <a name="monitor-applications"></a>Övervakning av program

Om din arbetsbelastning i Azure innehåller program som finns i [virtuella datorer (som skapats via Azure Resource Manager)](../azure-resource-manager/resource-manager-deployment-model.md) Security Center kan övervaka dessa tooidentify potentiella problem med exponerade webbportar (TCP-portarna 80 och 443) och rekommenderar steg. När du klickar på hello **program** panelen, hello **program** blad öppnas med ett antal rekommendationer i hello **programmet rekommendationer** avsnitt. Hello programmet uppdelning per värd för virtuell IP-adress visas också som visas i följande skärmbild hello.

![Programsäkerhetshälsa](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Precis som du med hello andra rekommendationer, du kan klicka på en rekommendation toosee mer information om hello problemet och hur tooremediate. hello hello följande bild visas ett exempel är ett program som identifierats som ett oskyddat webbprogram. När du väljer hello-program som klassats säker öppnas ett nytt blad med hello följande alternativ:

![Information om en app som inte är säker](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Det här bladet har en lista över alla rekommendationer för det här programmet. När du klickar på hello **lägga till en brandvägg för webbaserade program** rekommendation, hello **lägga till en brandvägg för webbaserade program** blad öppnas med alternativ för du tooinstall en brandvägg för webbaserade program (Brandvägg) från en partner som visas i följande skärmbild hello.

![Dialogrutan Lägg till en brandvägg för webbaserade program](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Se även
I den här artikeln har du lärt dig hur toouse övervakningsfunktionerna i Azure Security Center. toolearn mer om Azure Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md): Lär dig hur tooconfigure säkerhetsinställningar i Azure Security Center.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md): Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md): Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md): finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/): Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
