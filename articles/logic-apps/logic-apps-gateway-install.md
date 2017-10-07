---
title: aaaInstall lokala datagateway - Azure Logic Apps | Microsoft Docs
description: "Innan du komma åt datakällor lokalt installera hello lokala datagateway för snabb överföring och kryptering mellan datakällor lokalt och logikappar"
keywords: "komma åt data på lokala, dataöverföring, kryptering och datakällor"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 01386a904d856ff545f2eca8eb1b008dcdc08574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-on-premises-data-gateway-for-azure-logic-apps"></a>Installera hello lokala datagateway för Azure Logic Apps

Innan dina logic apps kan komma åt datakällor lokalt, måste du installera och konfigurera hello lokala datagateway. hello gateway fungerar som en brygga som ger snabb överföring och kryptering mellan lokala system och dina logic apps. hello gateway vidarebefordrar data från lokala datakällor på krypterade kanaler via hello Azure Service Bus. All trafik som kommer från som säkra utgående trafik från hello gateway agent. Lär dig mer om [hur hello datagateway fungerar](#gateway-cloud-service).

hello gateway har stöd för anslutningar toothese datakällor lokalt:

*   BizTalk Server 2016
*   DB2  
*   Filsystem
*   Informix
*   MQ
*   MySQL
*   Oracle-databas
*   PostgreSQL
*   SAP-programserver 
*   SAP Message Server
*   SharePoint
*   SQL Server
*   Teradata

Dessa steg visar hur toofirst installera hello lokala datagateway innan du [upprätta en anslutning mellan hello gateway och dina logic apps](./logic-apps-gateway-connection.md). Läs mer om kopplingar som stöds, [kopplingar för Azure Logikappar](https://docs.microsoft.com/azure/connectors/apis-list). 

Information om hur toouse hello gateway med andra tjänster finns i följande artiklar:

*   [Microsoft Power BI lokala datagateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Azure Analysis Services lokala datagateway](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow lokala datagateway](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps lokala datagateway](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a>Krav

**Minsta**:

* 4.5 för .NET framework
* 64-bitars version av Windows 7 eller Windows Server 2008 R2 (eller senare)

**Rekommenderade**:

* 8 kärnor CPU
* 8 GB minne
* 64-bitarsversionen av Windows 2012 R2 (eller senare)

**Tänk på följande**:

* Installera hello lokala datagateway på en lokal dator.
Du kan inte installera hello gateway på en domänkontrollant.

   > [!TIP]
   > Det finns inte tooinstall hello gateway på hello samma dator som datakällan. toominimize latens, kan du installera hello gateway så nära som möjligt tooyour datakälla, eller på hello samma dator, förutsatt att du har behörighet.

* Installera inte hello gateway på en dator som stängs av, går toosleep eller inte ansluta toohello Internet eftersom hello gateway inte kan köras under dessa omständigheter. Gateway-prestanda kan dessutom lidande över ett nätverk.

* Under installationen måste du logga in med en [arbets- eller skolkonto](https://docs.microsoft.com/azure/active-directory/sign-up-organization) som hanteras av Azure Active Directory (Azure AD), inte ett Microsoft-konto. 

  Du har toouse hello samma arbets- eller skolkonto konto senare i hello Azure portal när du skapar och associerar en gateway-resurs med gatewayinstallationen. Du kan sedan välja den här gatewayresursen när du skapar hello anslutning mellan logik appen och hello lokala datakällan. [Varför måste använda en Azure AD-arbets eller skolkonto?](#why-azure-work-school-account)

  > [!TIP]
  > Om du har registrerat dig för ett erbjudande för Office 365 och gick inte att få din faktiska arbetet i e-post, din adress för inloggningen kan se ut som jeff@contoso.onmicrosoft.com. 

* Om du har en befintlig gateway som konfigurerats med ett installationsprogram som är äldre än version 14.16.6317.4 kan ändra du inte plats för din gateway körs hello senaste Installer. Du kan dock använda hello senaste installer tooset upp en ny gateway med hello plats i stället.
  
  Om du har en gateway-installationsprogram som är äldre än version 14.16.6317.4, men du inte har installerat din gateway ännu, kan du hämta och använda hello senaste installer.

<a name="install-gateway"></a>

## <a name="install-hello-data-gateway"></a>Installera hello datagateway

1.  [Hämta och kör installationsprogrammet för hello-gateway på en lokal dator](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2. Granska och Godkänn hello användningsvillkoren och sekretesspolicyn.

3. Ange hello sökväg på den lokala datorn där du vill att tooinstall hello gateway.

4. När du uppmanas logga in med ditt Azure arbets- eller skolkonto inte ett Microsoft-konto.

   ![Logga in med Azure arbets- eller skolkonto](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Nu registrera din installerade gateway med hello [gateway-Molntjänsten](#gateway-cloud-service). Välj **registrera en ny gateway på den här datorn**.

   hello gateway-Molntjänsten krypterar och lagrar dina autentiseringsuppgifter för datakälla och gateway-information. 
   hello service skickar också frågor och deras resultat mellan din logikapp, hello lokala datagateway och datakällan lokalt.

6. Ange ett namn för din gateway-installation. Skapa en återställningsnyckel och sedan bekräfta din återställningsnyckeln. 

   > [!IMPORTANT] 
   > Återställningsnyckeln måste innehålla minst åtta tecken. Kontrollera att du sparar och hålla hello nyckel på en säker plats. Du måste också den här nyckeln om du vill toomigrate, återställa eller ta över en befintlig gateway.

   1. Välj toochange hello standardregion för hello gateway-Molntjänsten och Azure Service Bus som används av din gateway-installation **ändra Region**.

      ![Ändra region](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      hello standardregion är hello region som är associerade med Azure AD-klienten.

   2. Öppna hello på hello nästa ruta, **väljer Region** för välja en annan region.

      ![Välj en annan region](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      Till exempel du kanske hello Välj samma region som din logikapp eller välj hello region närmaste tooyour lokala datakällan så att du kan minska svarstiden. Gateway-resurs och logik appen kan ha olika platser.

      > [!IMPORTANT]
      > Du kan inte ändra den här regionen efter installationen. Den här regionen avgör också och begränsar hello plats där du kan skapa hello Azure-resurs för din gateway. Så när du skapar din gateway-resurs i Azure, se till att hello resursplats matchar hello region som du valde under gatewayinstallationen av.
      > 
      > Om du vill toouse en annan region för din gateway senare, måste du ställa in en ny gateway.

   3. När du är klar kan du välja **klar**.

7. Nu följer du stegen i hello Azure-portalen så att du kan [skapa en Azure-resurs för din gateway](../logic-apps/logic-apps-gateway-connection.md). 

Lär dig mer om [hur hello datagateway fungerar](#gateway-cloud-service).

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a>Migrera, Återställ eller ta över en befintlig gateway

tooperform dessa uppgifter, måste du ha hello återställningsnyckeln som angavs när hello gateway har installerats.

1. Datorns Start-menyn, Välj **lokala datagateway**.

2. När hello installer öppnas, logga in med hello samma Azure arbets- eller skolkonto som tidigare var används tooinstall hello gateway.

3. Välj **migrera, Återställ eller ta över en befintlig gateway**.

4. Ange hello namn och återställa nyckeln för hello-gateway som du vill ha toomigrate, Återställ eller ta över.

<a name="restart-gateway"></a>
## <a name="restart-hello-gateway"></a>Starta om hello gateway

hello gateway körs som en Windows-tjänst. Precis som alla andra Windows-tjänster, kan du starta och stoppa hello-tjänsten på flera olika sätt. Du kan till exempel öppna en kommandotolk med förhöjd behörighet på hello datorn där hello gatewayen körs och kör antingen dessa kommandon:

* toostop hello-tjänst, köra det här kommandot:
  
    `net stop PBIEgwService`

* toostart hello-tjänst, köra det här kommandot:
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a>Windows-tjänstkontot

hello lokala datagateway har konfigurerats toouse `NT SERVICE\PBIEgwService` tjänsten inloggningsuppgifter för Windows hello. Som standard har hello gateway hello ”logga in som en tjänst” höger för hello datorn där du installerar hello gateway.

> [!NOTE]
> hello Windows-tjänstkontot skiljer sig från hello-konto som används för att ansluta tooon lokala datakällor och hello Azure arbets eller skolkonto används toosign i toocloud services.

## <a name="configure-a-firewall-or-proxy"></a>Konfigurera en brandvägg eller proxyserver

hello gateway skapar en utgående anslutning för [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). information om tooprovide proxy för din gateway finns [konfigurera proxyinställningar](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).

toocheck om din brandvägg eller proxyserver, kan blockera anslutningar, bekräfta om datorn faktiskt kan ansluta toohello internet och hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Kör kommandot från en PowerShell-kommandotolk:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Det här kommandot kan endast test nätverksanslutning och anslutningen toohello Azure Service Bus. Så hello-kommandot har ingenting toodo med hello gateway eller hello gateway-Molntjänsten som krypterar och lagrar dina autentiseringsuppgifter och gateway-information. 
>
> Det här kommandot är också bara tillgängligt på Windows Server 2012 R2 eller senare och Windows 8.1 eller senare. I tidigare versioner av Operativsystemet, kan du använda Telnet för testa anslutningen. Lär dig mer om [Azure Service Bus och hybridlösningar](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

Resultatet bör se ut ungefär toothis exempel:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Om **TcpTestSucceeded** har inte angetts för**SANT**, du kan blockeras av en brandvägg. Om du vill toobe omfattande ersätta hello **ComputerName** och **Port** värden med hello värden som visas under [konfigurera portar](#configure-ports) i det här avsnittet.

hello-brandväggen kan också blockera anslutningar som hello Azure Service Bus gör toohello Azure-datacenter. Om det här scenariot inträffar kan godkänna (avblockera) alla hello IP-adresser för de datacenter i din region. För de IP-adresserna [get hello Azure IP-adresslistan här](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Konfigurera portar

hello gateway skapar en utgående anslutning för[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) och kommunicerar på utgående portar: TCP 443 (standard), 5671, 5672, 9350 via 9354. hello gateway kräver inte ingående portar. Lär dig mer om [Azure Service Bus och hybridlösningar](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMÄNNAMN | UTGÅENDE PORTAR | BESKRIVNING |
| --- | --- | --- |
| *. analysis.windows.net | 443 | HTTPS | 
| *. login.windows.net | 443 | HTTPS | 
| *. servicebus.windows.net | 5671-5672 | Avancerade Message Queuing-protokollet (AMQP) | 
| *. servicebus.windows.net | 443, 9350-9354 | Lyssnare på Service Bus Relay via TCP (kräver 443 för åtkomstkontroll token) | 
| *. frontend.clouddatahub.net | 443 | HTTPS | 
| *. core.windows.net | 443 | HTTPS | 
| login.microsoftonline.com | 443 | HTTPS | 
| *. msftncsi.com | 443 | Använda tootest internet-anslutning när hello gateway inte kan nås av hello Power BI-tjänsten. | 

Om du har tooapprove IP-adresser i stället för hello domäner, du kan hämta och använda hello [Microsoft Azure Datacenter IP-intervall lista](https://www.microsoft.com/download/details.aspx?id=41653). I vissa fall görs hello Azure Service Bus-anslutningar med IP-adress i stället för fullständigt kvalificerade domännamn.

<a name="gateway-cloud-service"></a>
## <a name="how-does-hello-data-gateway-work"></a>Hur fungerar hello datagateway?

Hej datagatewayen underlättar snabbt och säkert kommunikationen mellan din logikapp, hello gateway-Molntjänsten och lokala datakällan. 

![diagram-for-on-premises-data-gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Så när hello användaren i hello molnet interagerar med ett element som har anslutit tooyour lokala datakällan:

1. hello gateway-Molntjänsten skapar en fråga, tillsammans med hello krypterade autentiseringsuppgifter för datakälla för hello, och skickar hello frågan toohello kön för hello gateway tooprocess.

2. hello gateway-Molntjänsten analyserar hello frågan och skickar hello begäran toohello Azure Service Bus.

3. hello lokala datagateway avsöker hello Azure Service Bus för väntande begäranden.

4. hello gateway hämtar hello frågan dekrypterar hello autentiseringsuppgifter och ansluter toohello datakällan med autentiseringsuppgifterna.

5. hello gateway skickar hello toohello frågedatakällan för körning.

6. hello resultat skickas från hello datakällan tillbaka toohello gateway och sedan toohello gateway-Molntjänsten. hello använder gateway-Molntjänsten sedan hello resultat.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="general"></a>Allmänt

**Q**: behöver jag en gateway för datakällor i hello molnet, till exempel SQL Azure? <br/>
**En**: Nej En gateway ansluter bara tooon lokala datakällor.

**Q**: har hello gateway toobe installerad på detsamma datorn som datakälla för hello hello? <br/>
**En**: Nej hello gateway ansluter toohello datakälla med hjälp av hello anslutningsinformationen som angavs. Överväg att hello gateway som ett klientprogram på detta sätt. hello gateway måste bara hello kapaciteten tooconnect toohello namn på servern som har angetts.

<a name="why-azure-work-school-account"></a>

**Q**: Varför måste jag använda en Azure arbete eller skola konto toosign i? <br/>
**En**: du kan bara använda en Azure arbets- eller skolkonto när du installerar hello lokala datagateway. Din inloggning konto lagras i en klient som hanteras av Azure Active Directory (AD Azure). Azure AD-kontot användarens huvudnamn (UPN) matchar vanligtvis hello e-postadress.

**Q**: var lagras mina autentiseringsuppgifter? <br/>
**En**: hello-autentiseringsuppgifter som du anger för en datakälla krypteras och lagras i hello gateway-Molntjänsten. hello autentiseringsuppgifter dekrypteras på hello lokala datagateway.

**Q**: finns det några krav för nätverksbandbredd? <br/>
**En**: Vi rekommenderar att nätverksanslutningen har bra genomströmning. Alla miljöer skiljer sig, och hello mängden data som skickas påverkar hello resultat. Med hjälp av ExpressRoute kan hjälpa tooguarantee en nivå av dataflödet mellan lokala och hello Azure-datacenter.
Du kan använda hello verktyg från tredje part hastighet testning i Azure app toohelp mätaren din genomflöde.

**Q**: Vad är hello svarstid för att köra frågor tooa datakällan från hello gateway? Vad är bästa hello-arkitektur? <br/>
**En**: tooreduce Nätverksfördröjningen, installera hello gateway som Stäng toohello datakälla som möjligt. Om du kan installera hello gateway på hello faktiska datakällan, minimerar den här närhet hello latens införs. Överväg att hello Datacenter för. Till exempel om din tjänst använder hello västra USA datacenter, och du har SQL Server finns i en Azure VM, ska Azure VM vara i hello västra USA för. Den här närhet minimerar fördröjning och undviker utgång avgifter för hello Azure VM.

**Q**: hur är skickade tillbaka toohello molnet? <br/>
**En**: resultat skickas via hello Azure Service Bus.

**Q**: finns det några inkommande anslutningar toohello gateway från hello molnet? <br/>
**En**: Nej hello gateway använder utgående anslutningar tooAzure Service Bus.

**Q**: Vad händer om jag blockera utgående anslutningar? Vad gör jag behöver tooopen? <br/>
**En**: Se hello portar och värdar som hello gateway används.

**Q**: det faktiska Windows hello-tjänsten som kallas?<br/>
**En**: tjänster, hello gateway kallas i Power BI Enterprise Gateway-tjänsten.

**Q**: kan hello gateway Windows-tjänsten körs med ett Azure Active Directory-konto? <br/>
**En**: Nej hello Windows-tjänst måste ha ett giltigt Windows-konto. Som standard körs hello-tjänsten med hello tjänst-SID NT SERVICE\PBIEgwService.

### <a name="high-availability-and-disaster-recovery"></a>Hög tillgänglighet och haveriberedskap

**Q**: vilka alternativ som är tillgängliga för katastrofåterställning? <br/>
**En**: du kan använda hello recovery viktiga toorestore eller flytta en gateway. När du installerar hello gateway, ange hello återställningsnyckeln.

**Q**: Vad är hello fördelen hello återställningsnyckeln? <br/>
**En**: hello återställningsnyckeln ger ett sätt toomigrate eller återställa gatewayinställningarna efter en katastrof.

**Q**: finns det några planer för att aktivera scenarier med hög tillgänglighet med hello gateway? <br/>
**En**: dessa scenarier är hello översikt, men vi ännu inte har en tidslinje.

## <a name="troubleshooting"></a>Felsökning

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**Q**: hur kan jag se vilka frågor skickas toohello lokala datakällan? <br/>
**En**: du kan aktivera frågespårning, vilket innefattar hello frågor som skickas. Kom ihåg toochange frågan spårning tillbaka toohello ursprungliga värdet när du är klar felsökning. Lämna frågespårning aktiverad skapar större loggar.

Du kan också titta på Verktyg som datakällan har för spårning frågor. Du kan till exempel använda Extended Events eller SQL Profiler för SQL Server och Analysis Services.

**Q**: där är hello gateway-loggarna? <br/>
**En**: Se verktyg senare i det här avsnittet.

### <a name="update-toohello-latest-version"></a>Uppdatera toohello senaste versionen

Många problem kan ansluta när hello gatewayversionen blir inaktuella. Kontrollera att du använder hello senaste versionen som allmän bra. Om du inte har uppdaterat hello gateway för en månad eller längre, kan du Överväg att installera hello senaste versionen av hello gateway och se om du kan återskapa hello problemet.

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a>Fel: Det gick inte tooadd användaren toogroup. (-2147463168 PBIEgwService användare)

Du kan få detta fel om du försöker tooinstall hello gateway på en domänkontrollant, vilket inte stöds. Se till att du distribuerar hello gateway på en dator som inte är en domänkontrollant.

## <a name="tools"></a>Verktyg

### <a name="collect-logs-from-hello-gateway-configurer"></a>Samla in loggar från hello gateway configurer

Du kan samla in flera loggar för hello gateway. Börja alltid med hello loggar!

#### <a name="installer-logs"></a>Installationsloggarna

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Av konfigurationsloggar

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise gateway tjänstloggar

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Händelseloggar

Du kan hitta hello loggar Data Management Gateway och PowerBIGateway under **program- och tjänstloggar**.

### <a name="fiddler-trace"></a>Fiddler spårning

[Fiddler](http://www.telerik.com/fiddler) är ett kostnadsfritt verktyg från Telerik som övervakar HTTP-trafik. Du kan se den här trafiken med hello Power BI-tjänsten från hello klientdatorn. Den här tjänsten kan visa fel och annan relaterad information.

## <a name="next-steps"></a>Nästa steg
    
* [Ansluta tooon lokala data från logikappar](../logic-apps/logic-apps-gateway-connection.md)
* [Enterprise integration-funktioner](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Kopplingar för Azure Logic Apps](../connectors/apis-list.md)
