---
title: aaaConnect datorer tooOMS med hello OMS Gateway | Microsoft Docs
description: "Anslut din OMS-hanterade enheter och datorer som övervakas av Operations Manager med hello OMS toosend data toohello OMS gatewaytjänsten när de inte har tillgång till Internet."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: magoedte
ms.openlocfilehash: 0cfa8f2fb66016e494f22c780e328be472b5fdee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-computers-without-internet-access-toooms-using-hello-oms-gateway"></a>Ansluta datorer utan Internet access tooOMS med hello OMS-Gateway

Det här dokumentet beskriver hur din OMS-hanterad och System Center Operations Manager övervakade datorer kan skicka data toohello OMS-tjänsten när de inte har tillgång till Internet. hello OMS-gatewayen, som är en vanlig HTTP-proxy som stöder HTTP-tunnel kan kommandot hello HTTP ansluta, samla in data och skicka den toohello OMS-tjänsten för deras räkning.  

hello OMS Gateway har stöd för:

* Azure Automation Hybrid Runbook Worker  
* Windows-datorer med hello Microsoft Monitoring Agent direktanslutna tooan OMS-arbetsyta
* Linux-datorer med hello OMS-Agent för Linux direktanslutna tooan OMS-arbetsyta  
* System Center Operations Manager 2012 SP1 med UR7, Operations Manager 2012 R2 UR3 eller Operations Manager 2016 hanteringsgruppen integrerad med OMS.  

Om din IT-säkerhetsprinciper tillåter inte datorer på ditt nätverk tooconnect toohello Internet, till exempel försäljning (POS) enheter eller servrar som stöder IT-tjänster, men du behöver tooconnect dem tooOMS toomanage och övervaka dem, de kan konfigureras toocommunicate direkt med hello OMS tooreceive gatewaykonfigurationen och vidarebefordra data åt.  Om dessa datorer är konfigurerade med hello OMS-agenten toodirectly ansluta tooan OMS-arbetsyta, alla datorer i stället kommunicerar med hello OMS-Gateway.  hello gateway överför data från hello agenter tooOMS direkt, analyserar alla hello data under överföring inte.

När en hanteringsgrupp för Operations Manager är integrerat med OMS kan hello hanteringsservrar konfigurerade tooconnect toohello OMS Gateway tooreceive konfigurationsinformation och skicka insamlade data beroende på hello-lösning som du har aktiverat.  Operations Manager-agenter skickar vissa data som Operations Manager-aviseringar, configuration assessment, instansutrymme och kapacitet data toohello management-servern. Andra stora volymer data, till exempel IIS-loggar, prestanda och säkerhetshändelser skickas direkt toohello OMS-Gateway.  Om du har en eller flera Operations Manager-Gateway-servrar distribueras i en DMZ eller andra isolerat nätverk toomonitor obetrodda system, går inte att det kommunicera med en OMS-Gateway.  Operations Manager-Gateway-servrar kan endast tooa hantering av rapportservern.  När en Operations Manager-hanteringsgrupp är konfigurerad toocommunicate med hello OMS-Gateway, hello proxy konfigurationsinformationen är automatiskt tooevery agenthanterad dator som är konfigurerade toocollect data för Log Analytics, även om hello-inställningen är tom.    

tooprovide hög tillgänglighet för direkt ansluten eller Operations Management-grupper som kommunicerar med OMS via hello gateway kan du använda Utjämning av nätverksbelastning tooredirect och distribuera hello trafik mellan flera gateway-servrar.  Om en gateway-servern kraschar, är hello trafik omdirigerade tooanother tillgängliga noden.  

Det rekommenderas att du installerar hello OMS-agent på hello hello OMS Gateway programvara toomonitor hello OMS Gateway-dator och analysera prestanda eller händelse. Dessutom identifiera hello agent hjälper hello OMS Gateway hello-tjänstens slutpunkter som toocommunicate med krävs.

Varje agent måste ha nätverksgateway anslutningen tooits så att agenterna kan överföra data tooand automatiskt från hello gateway. Installera hello gateway på en domänkontrollant rekommenderas inte.

hello följande diagram visar data flödar från direkt agenter tooOMS med hello gateway-servern.  Agenter måste ha sina matchning för konfiguration av proxy hello samma port hello OMS-Gateway är konfigurerade toocommunicate med tooOMS.  

![direkt agentkommunikation med OMS-diagram](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

hello visar följande diagram data flödar från ett Operations Manager management group tooOMS.   

![Operations Manager-kommunikation med OMS-diagram](./media/log-analytics-oms-gateway/oms-omsgateway-opsmgrconnect.png)

## <a name="prerequisites"></a>Krav

När du ställer in en dator toorun hello OMS-Gateway, måste den här datorn har hello följande:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET framework 4.5
* Minst en 4 kärnor och 8 GB minne

### <a name="language-availability"></a>Tillgängliga språk

hello OMS-Gateway finns i hello följande språk:

- Kinesiska (förenklad)
- Kinesiska (traditionell)
- Tjeckiska
- Nederländska
- Svenska
- Franska
- Tyska
- Ungerska
- Italienska
- Japanska
- Koreanska
- Polska
- Portugisiska (Brasilien)
- Portugisiska (Portugal)
- Ryska
- Spanska (internationell)

## <a name="download-hello-oms-gateway"></a>Hämta hello OMS-Gateway

Det finns tre sätt tooget hello senaste versionen av hello OMS Gateway installationsfilen.

1. Ladda ned från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Hämta från hello OMS-portalen.  När du loggar in tooyour OMS-arbetsytan navigera för**inställningar** > **anslutna källor** > **Windows-servrar** och klicka på **Hämta OMS Gateway**.

3. Ladda ned från hello [Azure-portalen](https://portal.azure.com).  När du loggar in:  

   1. Bläddra hello lista över tjänster och välj sedan **logganalys**.  
   2. Välj en arbetsyta.
   3. I arbetsytan-bladet under **allmänna**, klickar du på **Snabbstart**.
   4. Under **välja en tooconnect toohello arbetsytan för datakälla**, klickar du på **datorer**.
   5. I hello **direkt Agent** bladet, klickar du på **hämta OMS Gateway**.<br><br> ![Hämta OMS-Gateway](./media/log-analytics-oms-gateway/download-gateway.png)


## <a name="install-hello-oms-gateway"></a>Installera hello OMS-Gateway

tooinstall en gateway, utför hello följande steg.  Om du har installerat en tidigare version kallades *Log Analytics vidarebefordrare*, kommer att uppgraderas toothis versionen.  

1. Dubbelklicka på hello målmappen **OMS Gateway.msi**.
2. På hello **Välkommen** klickar du på **nästa**.<br><br> ![Installationsguiden för gateway](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br>
3. På hello **licensavtalet** väljer **jag accepterar hello villkoren i licensavtalet för hello** tooagree toohello LICENSAVTALET och klicka sedan på **nästa**.
4. På hello **Port och proxy adress** sidan:
   1. Antalet toobe för typen hello TCP-port används för hello gateway. Installationsprogrammet konfigurerar en inkommande regel med det här portnumret på Windows-brandväggen.  hello standardvärdet är 8080.
      hello giltiga intervallet i hello portnumret är 1-65535. Om hello indata inte hamnar i det här intervallet, visas ett felmeddelande.
   2. Alternativt hello servern där hello gatewayen är installerad måste toocommunicate via en proxyserver, skriver du hello proxyadress där hello gateway måste tooconnect. Till exempel `http://myorgname.corp.contoso.com:80`.  Om det är tomt, försöker hello gateway tooconnect toohello Internet direkt.  Om proxyservern kräver autentisering, ange ett användarnamn och lösenord.<br><br> ![Proxykonfiguration för gateway-guiden](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
   3. Klicka på **Nästa**.
5. Om du inte har Microsoft Update har aktiverats hello Microsoft Update visas där du kan välja tooenable den. Gör ett val och klicka sedan på **nästa**. I annat fall Fortsätt toohello nästa steg.
6. På hello **målmappen** sida, lämnar du antingen hello mappen C:\Program Files\OMS Gateway eller typen hello standardplatsen där du vill tooinstall gateway och klicka sedan på **nästa**.
7. På hello **klar tooinstall** klickar du på **installera**. User Account Control visas den begärande behörighet tooinstall. I så fall, klickar du på **Ja**.
8. När installationen är klar klickar du på **Slutför**. Du kan kontrollera att hello-tjänsten körs genom att öppna hello services.msc snapin-modulen och kontrollera att **OMS Gateway** visas i hello listan med tjänster och status är **kör**.<br><br> ![Tjänster – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Konfigurera Utjämning av nätverksbelastning
Du kan konfigurera hello gateway för hög tillgänglighet med hjälp av Utjämning av nätverksbelastning (NLB) med hjälp av antingen Microsoft NLB (Utjämning av nätverksbelastning) eller maskinvarubaserad belastningsutjämnare.  hello belastningsutjämnaren hanterar trafik genom att omdirigera hello begärda anslutningar från hello OMS-Agent eller Operations Manager-hanteringsservrar över dess noder. Om en Gateway-servern kraschar, hämtar hello trafik omdirigerade tooother noder.

toolearn hur toodesign och distribuera en Windows Server 2016 Utjämning av nätverksbelastning kluster, se [Utjämning av nätverksbelastning](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  hello följande steg beskriver hur tooconfigure ett Microsoft-nätverk att läsa in kluster.  

1.  Logga in på hello Windows server som är medlem i hello NLB-kluster med ett administratörskonto.  
2.  Öppna Hanteraren för Utjämning av nätverksbelastning i Serverhanteraren, klicka på **verktyg**, och klicka sedan på **hanteraren**.
3. tooconnect en OMS-Gateway-servern med hello Microsoft Monitoring Agent installerad, högerklicka på hello klustrets IP-adress och klicka sedan på **Lägg till värd tooCluster**.<br><br> ![Läsa in belastningsutjämning Nätverkshanteraren – Lägg till värd tooCluster](./media/log-analytics-oms-gateway/nlb02.png)<br>
4. Ange hello IP-adress som du vill tooconnect hello gateway-servern.<br><br> ![Läsa in belastningsutjämning Nätverkshanteraren – Lägg till värd tooCluster: ansluta](./media/log-analytics-oms-gateway/nlb03.png)

## <a name="configure-oms-agent-and-operations-manager-management-group"></a>Konfigurera OMS-agent och Operations Manager-hanteringsgruppen
hello innehåller följande avsnitt anvisningar om hur tooconfigure direktanslutna OMS agenter, en Operations Manager-hanteringsgrupp eller Azure Automation Hybrid Runbook Workers med hello OMS Gateway toocommunicate med OMS.  

toounderstand krav och anvisningar om hur tooinstall hello OMS-agent på Windows-datorer ansluta direkt tooOMS, se [ansluta Windows-datorer tooOMS](log-analytics-windows-agents.md) eller Linux-datorer finns i [ansluta Linux-datorer tooOMS](log-analytics-linux-agents.md).

### <a name="configuring-hello-oms-agent-and-operations-manager-toouse-hello-oms-gateway-as-a-proxy-server"></a>Konfigurera hello OMS-agent och Operations Manager toouse hello OMS-Gateway som en proxyserver

### <a name="configure-standalone-oms-agent"></a>Konfigurera fristående OMS-agent
Se [konfigurera proxy-och brandväggsinställningarna med hello Microsoft Monitoring Agent](log-analytics-proxy-firewall.md) information om hur du konfigurerar en agent toouse en proxyserver i det här fallet är hello gateway.  Om du har distribuerat flera gateway-servrar bakom en belastningsutjämnare för nätverk är hello OMS proxykonfiguration hello virtuella IP-adressen för hello NLB:<br><br> ![Microsoft Monitoring agentegenskaper – proxyinställningar](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager---all-agents-use-hello-same-proxy-server"></a>Konfigurera Operations Manager - alla agenter använder hello samma proxyserver
Du kan konfigurera Operations Manager tooadd hello gateway-servern.  hello Operations Manager-proxykonfigurationen är automatiskt tillämpas tooall agenter som rapporterar tooOperations Manager, även om hello inställningen är tom.

toouse hello Gateway toosupport Operations Manager, måste du ha:

* Microsoft Monitoring Agent (agentversion – **8.0.10900.0** och senare) installerat på hello Gateway-servern och konfigurerade för hello OMS arbetsytor som du vill använda toocommunicate.
* hello gateway måste ha Internetanslutning eller vara anslutna tooa proxyserver som tillåter.

> [!NOTE]
> Om du inte anger ett värde för hello gateway är tomma värden pushas tooall agenter.


1. Öppna hello Operations Manager-konsolen och under **Operations Management Suite**, klickar du på **anslutning** och klicka sedan på **konfigurera proxyservern**.<br><br> ![Operations Manager – Konfigurera proxyserver](./media/log-analytics-oms-gateway/scom01.png)<br>
2. Välj **använder en proxy server tooaccess hello Operations Management Suite** och skriv sedan hello hello OMS Gateway-serverns IP-adress eller virtuella IP-adressen för hello NLB. Se till att du börjar med hello `http://` prefix.<br><br> ![Operations Manager – proxyserveradress](./media/log-analytics-oms-gateway/scom02.png)<br>
3. Klicka på **Slutför**. Operations Manager-servern är ansluten tooyour OMS-arbetsyta.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Konfigurera Operations Manager - specifik agenter använder för proxyserver
För stora eller komplexa miljöer, kan du bara vill att specifika servrar (eller grupper) toouse hello OMS Gateway-servern.  Du kan inte uppdatera hello Operations Manager agent direkt som det här värdet skrivs över av hello globalt värde för hello hanteringsgruppen för dessa servrar.  I stället måste toooverride hello regel som används toopush dessa värden.

> [!NOTE]
> Den här tekniken med samma konfiguration kan vara används tooallow hello användning av flera OMS Gateway-servrar i din miljö.  Du kan till exempel kräva specifika OMS Gateway-servrar toobe anges på grundval av per region.

1. Öppna hello Operations Manager-konsolen och välj hello **redigering** arbetsytan.  
2. I arbetsytan redigering hello väljer **regler** och klicka på hello **omfång** i verktygsfältet hello Operations Manager. Om den här knappen inte är tillgänglig, kontrollera att du har ett objekt, inte en mapp markerad i övervakningsfönstret hello toomake. Hej **omfång för Hanteringspaketsobjekt** dialogrutan visar en lista med vanliga riktade klasser, grupper eller objekt.
3. Typen **Hälsotjänsten** i hello **leta efter** fältet och markerar den hello-listan.  Klicka på **OK**.  
4. Sök efter hello regeln **Advisor Proxy inställningen regeln** och i hello Operations-konsolen i verktygsfältet klickar du på **åsidosätter** och peka för**åsidosättning hello Rule\For ett specifikt objekt i klassen: hälsa Tjänsten** och välj ett specifikt objekt hello-listan.  Alternativt kan du kan skapa en anpassad grupp som innehåller hello hälsa webbtjänstobjektet hello servrar du vill tooapply denna åsidosättning tooand och sedan använda hello åsidosättning toothat grupp.
5. I hello **egenskaper för åsidosättning** dialogrutan klickar du på tooplace hello markerat **åsidosätta** kolumnen nästa toohello **WebProxyAddress** parameter.  I hello **åsidosättningsvärde** anger hello URL hello OMS Gateway server att du börjar med hello `http://` prefix.
   >[!NOTE]
   > Du behöver inte tooenable hello regeln eftersom den redan hanteras automatiskt med en åsidosättning i hello Microsoft System Center Advisor säker referens Override hanteringspaket riktad hello övervakning servergrupp för Microsoft System Center Advisor.
   >
6. Välj antingen ett management pack från hello **väljer målhanteringspaket** listan eller skapa ett nytt oförseglat hanteringspaket genom att klicka på **ny**.
7. När du är klar med ändringarna klickar du på **OK**.

### <a name="configure-for-automation-hybrid-workers"></a>Konfigurera för automation-hybrider
Om du har Automation Hybrid Runbook Worker-arbeten i din miljö, hello följande ange manuell, tillfälligt lösningar tooconfigure hello Gateway toosupport dem.

I följande steg hello, behöver du tooknow hello Azure-region där hello Automation-kontot finns. toolocate hello plats:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj hello Azure Automation-tjänsten.
3. Välj hello lämplig Azure Automation-konto.
4. Visa dess region under **plats**.<br><br> ![Azure-portalen – platsen för Automation-konto](./media/log-analytics-oms-gateway/location.png)  

Använd följande tabeller tooidentify hello URL: en för varje plats hello:

**Jobbet runtime data service-URL: er**

| **Plats** | **URL: EN** |
| --- | --- |
| Norra centrala USA |ncus jobruntimedata-produktprenumeration-su1.azure-automation.net |
| Västra Europa |we-jobruntimedata-prod-su1.azure-automation.net |
| Södra centrala USA |scus-jobruntimedata-prod-su1.azure-automation.net |
| Östra USA 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Kanada central |cc-jobruntimedata-prod-su1.azure-automation.net |
| Norra Europa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Sydostasien |sea-jobruntimedata-prod-su1.azure-automation.net |
| Indien, centrala |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australien |ase-jobruntimedata-prod-su1.azure-automation.net |

**URL: er till Agent**

| **Plats** | **URL: EN** |
| --- | --- |
| Norra centrala USA |ncus agentservice-produktprenumeration-1.azure-automation.net |
| Västra Europa |Vi-agentservice-produktprenumeration-1.azure-automation.net |
| Södra centrala USA |scus agentservice-produktprenumeration-1.azure-automation.net |
| Östra USA 2 |eus2 agentservice-produktprenumeration-1.azure-automation.net |
| Kanada central |CC-agentservice-produktprenumeration-1.azure-automation.net |
| Norra Europa |ne agentservice-produktprenumeration-1.azure-automation.net |
| Sydostasien |SEA-agentservice-produktprenumeration-1.azure-automation.net |
| Indien, centrala |CID agentservice-produktprenumeration-1.azure-automation.net |
| Japan |jpe agentservice-produktprenumeration-1.azure-automation.net |
| Australien |ase agentservice-produktprenumeration-1.azure-automation.net |

Om datorn är registrerad som en Hybrid Runbook Worker automatiskt för korrigering hello Update lösning, gör du följande:

1. Lägg till hello jobbet Körningsdata tjänsten URL-toohello listan tillåtna värden på hello OMS-Gateway. Exempel: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Starta om hello OMS-Gateway-tjänsten med hjälp av hello följande PowerShell-cmdlet:`Restart-Service OMSGatewayService`

Om datorn är publicerat tooAzure Automation med cmdleten hello Hybrid Runbook Worker registrering, gör du följande:

1. Lägg till hello agent-tjänsten registrering URL toohello tillåtna värden lista på hello OMS-Gateway. Exempel: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Lägg till hello jobbet Körningsdata tjänsten URL-toohello listan tillåtna värden på hello OMS-Gateway. Exempel: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Starta om hello OMS-Gateway-tjänsten.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Användbar PowerShell-cmdlets
Cmdlets kan hjälpa dig att utföra uppgifter som är nödvändiga tooupdate hello OMS-gatewayens konfigurationsinställningar. Innan du använder dem, måste du:

1. Installera hello OMS Gateway (MSI).
2. Öppna ett PowerShell-konsolfönster.
3. tooimport hello modulen, Skriv följande kommando:`Import-Module OMSGateway`
4. Om inga fel uppstod i hello föregående steg, hello modulen har importerats och hello cmdlets kan användas. Typ`Get-Module OMSGateway`
5. Se till att du startar om hello Gateway-tjänsten när du gör ändringar med hello-cmdlets.

Om du får ett fel i steg 3 kan importera inte hello-modulen. hello fel kan inträffa när PowerShell är toofind hello modul. Du hittar i hello Gateway installationssökvägen: *C:\Program Files\Microsoft OMS Gateway\PowerShell*.

| **Cmdlet:** | **Parametrar** | **Beskrivning** | **Exempel** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Nyckel |Hämtar hello konfigurering av hello-tjänst |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Nyckel (krävs) <br> Värde |Ändringar hello konfigurering av hello-tjänst |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Hämtar hello-adress (överordnad) relay-proxy |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adress<br> Användarnamn<br> Lösenord |Anger hello-adress (och autentiseringsuppgifter) av relay (överordnad) proxy |1. Ange en relay-proxy och autentiseringsuppgifter:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Ange en relay-proxy som inte kräver autentisering:`Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Rensa hello relay proxyinställning:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Hämtar hello tillåtna värden (endast hello lokalt konfigurerade tillåtna värden inte inkluderas automatiskt hämtade tillåtna värdar) |`Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedHost` |Värd (krävs) |Lägger till hello värden toohello listan över tillåtna |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Värd (krävs) |Tar bort hello värden från listan över tillåtna hello |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Ämne (krävs) |Lägger till hello klienten certifikatet ämne toohello listan över tillåtna |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Ämne (krävs) |Tar bort hello klienten certifikatets ämne från listan över tillåtna hello |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Hämtar hello tillåtna klienten certifikatämnen (bara hello lokalt konfigurerad tillåtna ämnen, inkluderas inte automatiskt hämtade tillåtna ämnen) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Felsökning
toocollect händelser som loggats av hello gateway, behöver du tooalso hello OMS-agenten har installerats.<br><br> ![Loggboken – OMS Gateway logg](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS Gateway händelse-ID och beskrivningar**

hello hello följande tabell visar händelse-ID och beskrivningar för OMS Gateway logghändelser.

| **ID** | **Beskrivning** |
| --- | --- |
| 400 |Alla fel som inte har ett specifikt ID |
| 401 |Felaktig konfiguration. Till exempel: listenPort = ”text” i stället för ett heltal |
| 402 |Undantag vid tolkning TLS handshake-meddelanden |
| 403 |Fel för nätverk. Till exempel: Det går inte att ansluta tootarget server |
| 100 |Allmän information |
| 101 |Tjänsten har startats |
| 102 |Tjänsten har stoppats |
| 103 |Tog emot ett HTTP-ansluta kommando från klient |
| 104 |Inte ett HTTP-ansluta kommando |
| 105 |Målservern är inte i listan över tillåtna eller hello målport är inte säker port (443) <br> <br> Kontrollera att hello MMA agenten på Gateway-servern och hello medarbetare som kommunicerar med hello Gateway är ansluten toohello samma logganalys-arbetsytan. |
| 105 |FEL TcpConnection – ogiltig klientcertifikat: CN = Gateway <br><br> Se till att: <br>    <br> &#149; Du använder en Gateway med versionsnumret 1.0.395.0 eller större. <br> &#149; hello MMA agenten på Gateway-servern och hello agenter kommunicerar med hello Gateway är ansluten toohello samma logganalys-arbetsytan. |
| 106 |Någon anledning hello TLS-session är misstänkt och avvisade |
| 107 |hello TLS-sessionen har verifierats |

**Prestandaräknare toocollect**

hello visar följande tabell hello prestandaräknare som är tillgängliga för hello OMS-Gateway. Du kan lägga till hello räknare med Prestandaövervakaren.

| **Namn** | **Beskrivning** |
| --- | --- |
| OMS Gateway/aktiv klientanslutning |Antal aktiva klientnätverksanslutningar (TCP) |
| OMS Gateway/Felräkning |Antal fel |
| OMS-Gateway/ansluten klient |Antal anslutna klienter |
| OMS Gateway-avvisande antal |Antal nekanden på grund av fel vid verifiering av tooany TLS |

![Prestandaräknare OMS-Gateway](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Få hjälp
När du är inloggad toohello Azure-portalen kan skapa du en begäran om du behöver hjälp med hello OMS-Gateway eller andra Azure-tjänst eller funktion i en tjänst.
toorequest hjälp hello frågetecken symbol i hello övre högra hörnet av hello portal och klicka sedan på **ny supportbegäran**. Slutför hello formulär för nya stöd.

![Ny supportbegäran](./media/log-analytics-oms-gateway/support.png)

Du kan också lämna feedback om OMS eller logganalys på hello [Microsoft Azure Feedbackforum](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Nästa steg
* [Lägg till datakällor](log-analytics-data-sources.md) toocollect data från hello anslutna källor i OMS-arbetsytan och lagra den på hello OMS-databasen.
